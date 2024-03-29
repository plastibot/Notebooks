# Build Tensorflow Micropython

If you were building from scratch before 2021-12-15 then follow the [Upgrade Instructions](UPGRADE.md).

If you need instructions on how to debug on ESP32 or STM32 on device using JTAG then follow the [On Device Debugging Instructions](DEBUGGING.md)

## Overview 
We use the Micropython USER_C_MODULES extension mechanism for building a firmware with the microlite module and ulab
included.

The tensorflow lite micro code is generated using the [tensorflow/tensorflow/lite/micro/tools/project_generation/create_tflm_tree.py](https://github.com/mocleiri/tflite-micro/blob/main/tensorflow/lite/micro/tools/project_generation/create_tflm_tree.py)
python script.

Depending on the port it works in conjunction with the ./tensorflow/lite/micro/tools/make/Makefile to generate the source files
for the tensorflow lite micro project suitable for inclusion in downstream projects.

Some ports like stm32 and rp2 have custom kernels that are provided in place of the reference kernels.

There is a per port script within the micropython-modules/microlite directory that will invoke this script with the correct
options.

* [prepare-tflm-esp.sh](https://github.com/mocleiri/tensorflow-micropython-examples/blob/main/micropython-modules/microlite/prepare-tflm-esp.sh)
* [prepare-tflm-rp2.sh](https://github.com/mocleiri/tensorflow-micropython-examples/blob/main/micropython-modules/microlite/prepare-tflm-rp2.sh)
* [prepare-tflm-stm32.sh](https://github.com/mocleiri/tensorflow-micropython-examples/blob/main/micropython-modules/microlite/prepare-tflm-stm32.sh)

Running the applicable script will create a tflm subdirectory within micropython-modules/microlite and it is under this
where all of the tensorflow c++ files are located.


## Building ESP32

Instructions for [building ESP32S3 on MAC OS](MAC_ESP32S3_BUILD.md).

Remember to have built mpy-cross before building the board itself.

### Prerequisites
pip3 install Pillow
pip3 install Wave
sudo apt install cmake

The esp-idf is provisioned through the micropython/tools/ci.sh script.  We tie into the same tooling used by
micropython to build esp32.

If you have the idf installed somewhere else you can use that instead.  The shells running the build steps just
need to have been configured by esp-idf/export.sh prior to running commands.

### Clone the repository
```shell
git clone https://github.com/mocleiri/tensorflow-micropython-examples.git
cd tensorflow-micropython-examples
```

###  Setup submodules

```shell
git submodule init
git submodule update --recursive
cd micropython
git submodule update --init lib/axtls
git submodule update --init lib/berkeley-db-1.xx
cd ..
```

### Generate Tensorflow Micro source files

```shell
echo "Regenerating microlite/tfm directory"
rm -rf ./micropython-modules/microlite/tflm

cd ./tensorflow

../micropython-modules/microlite/prepare-tflm-esp.sh

cd ..
```

### Build mpy-cross before building the board

```shell
#source ./micropython/tools/ci.sh && ci_esp32_idf44_setup
source ./micropython/tools/ci.sh && ci_esp32_setup_helper v4.3.1
source ./esp-idf/export.sh
cd ./micropython
echo "make -C mpy-cross V=1 clean all"
make -C mpy-cross V=1 clean all
```

### Build Board

We should be able to support all micropython boards for the esp32 port.  However, at the moment we have our own directory
boards/esp32 which a further subdirectory for the boards we build.

Issues are welcome to request that we support new boards.  

I'm considering implementing the building logic ontop of micropython's auto build logic which would allow a microlite
build for all boards supported by the ports supported.



```shell
$ source ./esp-idf/export.sh
$ cd boards/esp32/MICROLITE
$ rm -rf build
$ idf.py build
```

After the successful build the build directory will contain:
1. bootloader/bootloader.bin 
2. partition_table/partition-table.bin 
3. micropython.bin

These can then be flashed onto your board individually or you can unify them into a single firmware.bin with the following comand:

```shell
../../../micropython-modules/microlite/assemble-unified-image-esp.sh ../../../micropython/ports/esp32

bootloader  @0x001000    21584  (    7088 remaining)
partitions  @0x008000     3072  (    1024 remaining)
application @0x010000  2081232  (  540208 remaining)
total                  2146768
```

The resulting firmware.bin can be found on folder boards/esp32/MICROLITE/Build

To burn the firmware to the ESP32 using esptool first erase the flash

```shell
esptool.py --port /dev/ttyUSB0 erase_flash
```

then program

```shell
esptool.py --chip esp32 -port /dev/ttyUSB0 write_flash -z 0x1000 firmware.bin
```





