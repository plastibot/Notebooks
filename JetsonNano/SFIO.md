
# Special Function enablement (I2S, SPI, etc)

The 40-pin header on the Jetson Nano comes configured as 'GPIO' (General purpose I/O) by default with the exception of 
UART_2 (Pins 8, 10),  I2C_2 (Pins 3, 5) and I2C_1 (Pins 27, 28). In order to get SPI and/or I2S enable the pins need to be reconfigured to what is called 'SFIO' (Special Function I/O).

In orde rto reconfigure the pins, follow the instructions here:



## I2S

The I2S and AUDIO_MCLK are mapped to the following GPIOs:

| Name | Tegra name.bank | J41 Header Pin |
|------| --------------- | -------------- |
| AUDIO_MCLK | GPIO BB.00 | (pin #7) |
| DAP4_SCLK  | GPIO  J.07 | (pin #12) |
| DAP4_FS    | GPIO  J.04 | (pin #35) |
| DAP4_DIN   | GPIO  J.05 | (pin #38) |
| DAP4_DOUT  | GPIO  J.06 | (pin #40) |

You can check to see if the pins are configured for GPIO using the following command:

````bash
$ sudo grep "Name:\|J:\|BB:" /sys/kernel/debug/tegra_gpio
````

a response other than 0x00 for the CNF column means the pins are GPIO.

````bash
Name:Bank:Port CNF OE OUT IN INT_STA INT_ENB INT_LVL
 J: 2:1 f0 00 00 00 00 00 000000
BB: 6:3 01 00 00 00 00 00 000000
````

The GPIO 'CNF' register indicate whether a pin is a GPIO or SFIO. If the bit is set it is a GPIO. So for example, 
in the above the DAP4 (I2S_4) pins are all GPIO because the 'CNF' register has the value 0xf0 and the per the above
the DAP4 pins occupy the GPIO port J 4-7 which maps to bits 4-7 in the CNF register. Similarly the AUDIO_MCLK is 
configured as a GPIO because GPIO port BB has bit 0 set to 1. 

This is how it looks when the I2S_4 port is enabled

````bash
Name:Bank:Port CNF OE OUT IN INT_STA INT_ENB INT_LVL
 J: 2:1 00 00 00 00 00 00 000000
BB: 6:3 00 00 00 00 00 00 000000
````

## SPI

SPI2 is mapped to the following GPIOs:

| blinka pin | Name | Tegra name.bank | J41 Header Pin |
|------| -----------| ---- | -------------- |
| D24  | SPI_2_CS0  | GPIO  B.07 | (pin #18) |
| D25  | SPI_2_MISO | GPIO  B.05 | (pin #22) |
| D26  | SPI_2_MOSI | GPIO  B.04 | (pin #37) |
| D27  | SPI_2_SCK  | GPIO  B.06 | (pin #13) |
| D23  | SPI_2_CS1  | GPIO DD.00 | (pin #16) |

SPI1 is mapped  to the following GPIOs

| blinka pin | Name | Tegra name.bank | J41 Header Pin |
|------| -----------| ---- | -------------- |
| D9  | SPI_1_MISO | GPIO C.01 | (pin #21) |
| D10 | SPI_1_MOSI | GPIO C.00 | (pin #19) |
| D11 | SPI_1_SCK  | GPIO C.02 | (pin #23) |
| D7  | SPI_1_CS1  | GPIO C.04 | (pin #26) |
| D8  | SPI_1_CS0  | GPIO C.03 | (pin #24) |

You can check to see if the pins are configured for GPIO by ...

````bash
$ sudo grep "Name:\| C:\| B:\|DD:" /sys/kernel/debug/tegra_gpio
````
A response like this means SPI_2 is enabled but only for CS0 and SPI_1 is disabled.

````bash
Name:Bank:Port CNF OE OUT IN INT_STA INT_ENB INT_LVL
 B: 0:1 00 00 00 00 00 00 000000
 C: 0:2 1f 00 00 00 00 00 000000 
DD: 7:1 01 00 00 00 00 00 000000
````

In order for SPI_1 and SPI_2 to be enabled it should report:

````bash
Name:Bank:Port CNF OE OUT IN INT_STA INT_ENB INT_LVL
 B: 0:1 00 00 00 00 00 00 000000
 C: 0:2 00 00 00 00 00 00 000000 
DD: 7:1 00 00 00 00 00 00 000000
````


## UART

UART_2_TX and UART_2_RX are enabled by default however the RTS and CTS pins are not.
RTS and CTS pins are mapped to:

| blinka pin | Name | Tegra name.bank | J41 Header Pin |
|------| -----------| ---- | -------------- |
| D16 | UART_2_CTS  | GPIO G.03 | (pin #36) |
| D17 | UART_2_RTS  | GPIO G.02 | (pin #11) |

You can check to see if the pins are configured for GPIO using the following command:

````bash
$ sudo grep "Name:\|G:" /sys/kernel/debug/tegra_gpio
````
The response below shows that both CTS and RTS pins are GPIO

````bash
Name:Bank:Port CNF OE OUT IN INT_STA INT_ENB INT_LVL
 G: 1:2 0c 00 00 04 00 00 000000
 ````
 
 in order for them to be enable as SFIO pins, the response should be:
 
 ````bash
Name:Bank:Port CNF OE OUT IN INT_STA INT_ENB INT_LVL
 G: 1:2 00 00 00 04 00 00 000000
 ````


## Pins left over as GPIO after assigning special functions:

15, 29, 31, 32, 33


## Reference



![J41_Pinout](https://github.com/plastibot/Notebooks/blob/master/JetsonNano/Jetson_Nano_J41_Pinout.png "J41 Pinout")

```` bash
# jetson nano board definition: https://github.com/adafruit/Adafruit_Blinka/blob/master/src/adafruit_blinka/board/jetson_nano.py

# note: the gpio pin number used in adafruit blinka python library is to follow the Raspberry Pi GPIO naming system not the pin sequence on board 
# - i2c pins: (can not be used as gpio)
# SDA = pin.SDA_1 (pin #3)
# SCL = pin.SCL_1 (pin #5)
# SDA_1 = pin.SDA (pin #27)
# SCL_1 = pin.SCL (pin #28)

# - uart pins: (not defined yet as of 2019-04-09. can not be used as gpio)
# TXD0 = pin.GPIO14 (pin #8)
# RXD0 = pin.GPIO15 (pin #10)

# - spi/i2s: nil

# - gpio pins:
# D4 = pin.BB00 (pin #7)
# D5 = pin.S05 (pin #29)
# D6 = pin.Z00 (pin #31)
# D7 = pin.C04 (pin #26)
# D8 = pin.C03 (pin #24)
# D9 = pin.C01 (pin #21)
# D10 = pin.C00 (pin #19)
# D11 = pin.C02 (pin #23)
# D12 = pin.V00 (pin #32)
# D13 = pin.E06 (pin #33)
# D16 = pin.G03 (pin #36)
# D17 = pin.G02 (pin #11)
# D18 = pin.J07 (pin #12)
# D19 = pin.J04 (pin #35)
# D20 = pin.J05 (pin #38)
# D21 = pin.J06 (pin #40)
# D22 = pin.Y02 (pin #15)
# D23 = pin.DD00 (pin #16)
# D24 = pin.B07 (pin #18)
# D25 = pin.B05 (pin #22)
# D26 = pin.B04 (pin #37)
# D27 = pin.B06 (pin #13)

# To use 2nd i2c bus at GPIO pin 27(SDA_1)/28(SCL_1) on Jetson Nano:
# - connect i2c sensor/oled i2c_sda --> jetson nano pin#27(sda_1)
# - connect i2c sesnor/oled i2c_scl --> jetson nano pin#28(scl_1)
# - connect i2c sensor/oled vcc(3v3) --> jetson nano pin#17(3v3) or pin#2/4 if the sensor/oled board is 5v/3.3v compatible
# - connect i2c sesnor/oled gnd --> jetson nano pin#14(gnd) or any available gnd pins on board

'''
ref: ~/jetbot/.local/lib/python3.6/site-packages/Jetson/GPIO/gpio_pin_data.py:
JETSON_NANO_PIN_DEFS = [
# (tegra210_pin, <-> , brd_pin#, rpi_pin_name, <->, <->) 
(216, "/sys/devices/6000d000.gpio", 7, 4, 'AUDIO_MCLK', 'AUD_MCLK'),
(50, "/sys/devices/6000d000.gpio", 11, 17, 'UART2_RTS', 'UART2_RTS'),
(79, "/sys/devices/6000d000.gpio", 12, 18, 'DAP4_SCLK', 'DAP4_SCLK'),
(14, "/sys/devices/6000d000.gpio", 13, 27, 'SPI2_SCK', 'SPI2_SCK'),
(194, "/sys/devices/6000d000.gpio", 15, 22, 'LCD_TE', 'LCD_TE'),
(232, "/sys/devices/6000d000.gpio", 16, 23, 'SPI2_CS1', 'SPI2_CS1'),
(15, "/sys/devices/6000d000.gpio", 18, 24, 'SPI2_CS0', 'SPI2_CS0'),
(16, "/sys/devices/6000d000.gpio", 19, 10, 'SPI1_MOSI', 'SPI1_MOSI'),
(17, "/sys/devices/6000d000.gpio", 21, 9, 'SPI1_MISO', 'SPI1_MISO'),
(13, "/sys/devices/6000d000.gpio", 22, 25, 'SPI2_MISO', 'SPI2_MISO'),
(18, "/sys/devices/6000d000.gpio", 23, 11, 'SPI1_SCK', 'SPI1_SCK'),
(19, "/sys/devices/6000d000.gpio", 24, 8, 'SPI1_CS0', 'SPI1_CS0'),
(20, "/sys/devices/6000d000.gpio", 26, 7, 'SPI1_CS1', 'SPI1_CS1'),
(149, "/sys/devices/6000d000.gpio", 29, 5, 'CAM_AF_EN', 'CAM_AF_EN'),
(200, "/sys/devices/6000d000.gpio", 31, 6, 'GPIO_PZ0', 'GPIO_PZ0'),
(168, "/sys/devices/6000d000.gpio", 32, 12, 'LCD_BL_PWM', 'LCD_BL_PW'),
(38, "/sys/devices/6000d000.gpio", 33, 13, 'GPIO_PE6', 'GPIO_PE6'),
(76, "/sys/devices/6000d000.gpio", 35, 19, 'DAP4_FS', 'DAP4_FS'),
(51, "/sys/devices/6000d000.gpio", 36, 16, 'UART2_CTS', 'UART2_CTS'),
(12, "/sys/devices/6000d000.gpio", 37, 26, 'SPI2_MOSI', 'SPI2_MOSI'),
(77, "/sys/devices/6000d000.gpio", 38, 20, 'DAP4_DIN', 'DAP4_DIN'),
(78, "/sys/devices/6000d000.gpio", 40, 21, 'DAP4_DOUT', 'DAP4_DOUT')
]
'''
````



