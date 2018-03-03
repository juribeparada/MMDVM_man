# Instalación de firmware MMDVM para Nucleo-64 F446RE

Si estás usando Pi-Star, expande el sistema de archivos (si no lo has realizado antes):

    sudo pistar-expand
    sudo reboot

Habilita la escritura en el disco si usas Pi-Star:

    rpi-rw

Actualizar la lista de paquetes:

    sudo apt-get update

Instala el compilador y paquetes necesarios:

    sudo apt-get install git gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib autoconf libtool pkg-config libusb-1.0-0 libusb-1.0-0-dev

Instala OpenOCD:

    git clone https://github.com/juribeparada/openocd
    cd openocd
    ./bootstrap
    ./configure
    make
    sudo make install

Baja el código fuente del firmware MMDVM:

    git clone https://github.com/g4klx/MMDVM
    cd MMDVM
    git clone https://github.com/juribeparada/STM32F4XX_Lib

Edita Config.h:

    nano Config.h
    
Debería quedar habilitado (sin "//" delante de cada linea) las siguientes opciones (para conector Morpho):

    #define ARDUINO_MODE_PINS
    #define STM32F4_NUCLEO_MORPHO_HEADER
    #define SEND_RSSI_DATA
    #define SERIAL_REPEATER
    #define USE_DCBLOCKER

Compila el código fuente:

    make nucleo

Si estas usando Pi-Star, hay que detener el servicio MMDVMHost para liberar el puerto serial:

    sudo pistar-watchdog.service stop
    sudo systemctl stop mmdvmhost.timer
    sudo systemctl stop mmdvmhost.service

Sube el firmware a la tarjeta MMDVM:

    sudo make deploy
