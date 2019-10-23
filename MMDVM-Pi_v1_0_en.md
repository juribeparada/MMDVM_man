# Enable serial port in Raspberry Pi 3 or Pi Zero W

This this necessary only if you are installing a fresh copy of Raspbian OS. Images like "Pi-Star" are already OK.

Edit /boot/cmdline.txt:

    sudo nano /boot/cmdline.txt

(remove the text: console=serial0,115200)

Disable services:

    sudo systemctl disable serial-getty@ttyAMA0.service
    sudo systemctl disable bluetooth.service

Edit /boot/config.txt:

    sudo nano /boot/config.txt

and add the following lines at the end of /boot/config.txt:

    enable_uart=1
    dtoverlay=pi3-disable-bt

Reboot your RPi:

    sudo reboot

# Build the firmware and upload to MMDVM-Pi

## Installation of necessary software (only once)

NOTE!!: To build firmware for the MMDVM-Pi v1.0 you need to use Pi-Star RPi_V4.1.0-RC4_27-Aug-2019 or higher. If you are using Raspbian, use Buster or higher

If you are using Pi-Star, expand filesystem (if you haven't done before):

    sudo pistar-expand
    sudo reboot

Enable RW filesystem if you are using Pi-Star:

    rpi-rw

Install toolchain and necessary packages:

    sudo apt-get install git gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib autoconf libtool pkg-config libusb-1.0-0 libusb-1.0-0-dev

If on Pi-Star Beta v4 you may get "cannot resolve host" error when downloading the packages. Do the following:

    sudo nano /etc/resolv.conf

Enter the following on a new line

    nameserver 8.8.8.8

Ctrl-O and Ctrl-X to save and exit. Now you can download the packages without getting that error.

Download and compile serial flashing utilities:

    cd ~
    git clone https://github.com/jsnyder/stm32ld
    cd stm32ld
    make
    sudo cp stm32ld /usr/local/bin

Remove libi2c-dev and stm32flash packages if you are using Pi-Star:

    sudo apt-get remove libi2c-dev
    sudo apt-get remove stm32flash

Install the latest stm32flash:

    cd ~
    git clone https://git.code.sf.net/p/stm32flash/code stm32flash
    cd stm32flash
    make
    sudo make install

Another place to get the lastest stm32flash is:

    https://sourceforge.net/projects/stm32flash/files/

## MMDVM firmware compilation and uploading for MMDVM-Pi

Download firmware sources:

    cd ~
    git clone https://github.com/g4klx/MMDVM
    cd MMDVM
    git submodule init
    git submodule update

Edit Config.h according your preferences:

    nano Config.h

You can select for example:

    // #define EXTERNAL_OSC 12000000 (disable any external TCXO)
    // #define ARDUINO_DUE_ZUM_V10 (this option doesn't matter for STM32 devices)
    #define MODE_LEDS
    #define SEND_RSSI_DATA
    #define SERIAL_REPEATER
    #define USE_DCBLOCKER
    #define USE_ALTERNATE_POCSAG_LEDS

Compile:

    make pi-f722

If you are using Pi-Star, stop services:

    sudo pistar-watchdog.service stop
    sudo systemctl stop mmdvmhost.timer
    sudo systemctl stop mmdvmhost.service

Upload the firmware:

    sudo make deploy-pi-f7

