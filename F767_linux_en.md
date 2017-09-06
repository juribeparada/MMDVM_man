# MMDVM firmware installation for Nucleo144 F767ZI

Install toolchain and necessary packages:

    sudo apt-get install git gcc-arm-none-eabi gdb-arm-none-eabi autoconf libtool pkg-config libusb-1.0-0 libusb-1.0-0-dev

Install OpenOCD:

    git clone https://github.com/ntfreak/openocd
    cd openocd
    ./bootstrap
    ./configure
    make
    sudo make install

Download the sources:

    git clone https://github.com/g4klx/MMDVM
    cd MMDVM
    git clone https://github.com/juribeparada/STM32F7XX_Lib

Edit Config.h according your preferences:

    nano Config.h

Compile the code:

    make f767

Upload the firmware:

    make deploy-f7
