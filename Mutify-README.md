Building modified firmware for ESP32
----------

**Required:**

- Micropython v1.20.0
- ESP-IDF v4.0.2

**Compiling Firmware**

First we need to prepare ESP-IDF
```shell
cd ~
git clone -b v4.0.2 --recursive https://github.com/espressif/esp-idf.git
cd esp-idf
./install.sh
source export.sh
```

Then we clone this micropython project and compile the firmware on branch `v1.20.0-more-ssl-ram`.

```shell
cd ~
git clone -b v1.20.0-more-ssl-ram git@github.com:Mutify/micropython.git
cd micropython
make -C mpy-cross
cd ports/esp32
make submodules
make
```

Flashing firmware

```shell
cd ~/micropython/ports/esp32
~/.espressif/python_env/idf4.0_py3.8_env/bin/python ~/esp-idf/components/esptool_py/esptool/esptool.py -p {PORT} -b 460800 --before default_reset --after hard_reset write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 build-GENERIC/bootloader/bootloader.bin 0x8000 build-GENERIC/partition_table/partition-table.bin 0x10000 build-GENERIC/micropython.bin 
```

Compiling for UNIX

```shell
cd ~
git clone -b v1.20.0-more-ssl-ram git@github.com:Mutify/micropython.git
cd micropython
make -C mpy-cross
cd ports/unix
make submodules
brew install libffi
CFLAGS="-I$(brew --prefix libffi)/include" make
```