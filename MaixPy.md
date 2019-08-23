## download the MaixPy firmware

Latest firmware binaries can be downloaded from:

http://dl.sipeed.com/MAIX/MaixPy/release/master/

or you can compile your own from here:

https://github.com/sipeed/MaixPy/releases


## Update the MaixPy firmware to the device





## Install Maix IDE

Download the latest from:

http://dl.sipeed.com/MAIX/MaixPy/ide/

Note that there are dependencies betweent the firmware level and the IDE level so make sure your Firmware suppports it. 
Read the history.txt under the general link and the readme.txt under the specific release


## Link to Maix documentation

https://maixpy.sipeed.com/



# Code Snippets for M5StickV

##  UART

```` python
from machine import UART
from fpioa_manager import fm
fm.register(34, fm.fpioa.UART_TX, force=True
fm.register(35, fm.fpioa.UART_RX, force=True
uart_A = UART (UART.UART1, 115200,8,0,0,timeout=1000, read_buf_len=4096)
uart_A.write("string"+"\r\n")
````

## Power management (AXP192)
```` python
import pmu

axp = pmu.axp192()
axp.enableADCs(True)

try:
  while(True):
    val = axp.getVbatVoltage()
    lcd.draw_string(0, 15, "Battery Voltage:" + str(val) + filler, lcd.RED, lcd.BLACK)

    val = axp.getUSBVoltage()
    lcd.draw_string(0, 30, "USB Voltage:" + str(val) + filler, lcd.WHITE, lcd.BLACK)

    val = axp.getUSBInputCurrent()
    lcd.draw_string(0, 45, "USB InputCurrent:" + str(val) + filler, lcd.RED, lcd.BLACK)

    val = axp.getBatteryDischargeCurrent()
    lcd.draw_string(0, 60, "DischargeCurrent:" + str(val) + filler, lcd.GREEN, lcd.BLACK)

    val = axp.getBatteryInstantWatts()
    lcd.draw_string(0, 75, "Instant Watts:" + str(val) + filler, lcd.BLUE, lcd.BLACK)

    val = axp.getTemperature()
    lcd.draw_string(0, 90, "Temperature:" + str(val) + filler, lcd.BLUE, lcd.BLACK)
    
````

## LCD Backlight control
````python
import lcd  #for test
from machine import I2C

i2c = I2C(I2C.I2C0, freq=400000, scl=28, sda=29)

lcd.init()                                                     #for test
lcd.draw_string(100, 100, "hello maixpy", lcd.RED, lcd.BLACK)  #for test

i2c.writeto_mem(0x34, 0x91,b'\x70')  # minimum
i2c.writeto_mem(0x34, 0x91,b'\xf0')  # maximum
````
