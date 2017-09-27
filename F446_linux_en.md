# MMDVM firmware installation for Nucleo64 F446RE

Enable RW filesystem if you are using Pi-Star:

    rpi-rw

Install toolchain and necessary packages:

    sudo apt-get install git gcc-arm-none-eabi gdb-arm-none-eabi autoconf libtool pkg-config libusb-1.0-0 libusb-1.0-0-dev

Install OpenOCD:

    git clone https://github.com/juribeparada/openocd
    cd openocd
    ./bootstrap
    ./configure
    make
    sudo make install

Download the sources:

    git clone https://github.com/g4klx/MMDVM
    cd MMDVM
    git clone https://github.com/juribeparada/STM32F4XX_Lib

Edit Config.h:

    nano Config.h
    
Usually you could enable (for Morpho connector):

    #define ARDUINO_MODE_PINS
    #define STM32F4_NUCLEO_MORPHO_HEADER
    #define SEND_RSSI_DATA
    #define SERIAL_REPEATER

Compile the code:

    make nucleo

Upload the firmware:

    sudo make deploy
