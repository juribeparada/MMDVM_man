# Habilitar puerto serial en Raspberry Pi 4 o Raspberry Pi 3 o Pi Zero W

Esto es necesario si estás usando una copia reciente de Raspbian OS. Imágenes como "Pi-Star" ya están listas y no es necesario este procedimiento.

Editar /boot/cmdline.txt:

    sudo nano /boot/cmdline.txt

(borrar el texto: console=serial0,115200)

Deshabitar servicios:

    sudo systemctl disable serial-getty@ttyAMA0.service
    sudo systemctl disable bluetooth.service

Editar /boot/config.txt:

    sudo nano /boot/config.txt

ya agregar las siguientes líneas al final del archivo /boot/config.txt:

    enable_uart=1
    dtoverlay=pi3-disable-bt

Reiniciar tu RPi:

    sudo reboot

# Compilar MMDVM firmware y subirlo al MMDVM-Pi

## Instalación de todo el software necesario (se realiza solamente una vez)

NOTA!!: Para poder compilar firmware para el MMDVM-Pi v1.0 necesitas usar Pi-Star RPi_V4.1.0-RC4_27-Aug-2019 o la version más reciente. Si vas a usar Rasbpian, utiliza la version Buster o la mas reciente.

Si estás usando Pi-Star, expande el sistema de archivos (si no lo has realizado antes):

    sudo pistar-expand
    sudo reboot

Habilita la escritura en el disco si usas Pi-Star:

    rpi-rw

Instala el compilador y paquetes necesarios:

    sudo apt-get install git gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib autoconf libtool pkg-config libusb-1.0-0 libusb-1.0-0-dev

Si estás usando Pi-Star Beta v4 quizas veas el error "cannot resolve host" al tratar de descargar los paquetes necesarios. Sigue los siguientes pasos:

    sudo nano /etc/resolv.conf

Pon lo siguiente en una nueva linea

    nameserver 8.8.8.8

Ctrl-O y Ctrl-X para guardar los cambios y salir del programa. Ahora podras descargar los paquetes necesarios sin el error.

Eliminar los paquetes libi2c-dev y stm32flash si estás usando Pi-Star:

    sudo apt-get remove libi2c-dev
    sudo apt-get remove stm32flash

Instalar la última versión de stm32flash:

    cd ~
    git clone https://git.code.sf.net/p/stm32flash/code stm32flash
    cd stm32flash
    make
    sudo make install

Puedes también bajar el zip con el código fuente y compilarlo a parte:

    https://sourceforge.net/projects/stm32flash/files/

## Instalación de firmware MMDVM para MMDVM-Pi

Baja el código fuente del firmware MMDVM:

    cd ~
    git clone https://github.com/g4klx/MMDVM
    cd MMDVM
    git submodule init
    git submodule update

Editar Config.h de acuerdo a tus preferencias:

    nano Config.h

Puedes seleccionar por ejemplo:

    // #define EXTERNAL_OSC 12000000 (deshabilitar todos los TCXO externos)
    // #define ARDUINO_DUE_ZUM_V10 (en realidad esta opción no importa en STM32)
    #define MODE_LEDS
    #define SEND_RSSI_DATA
    #define SERIAL_REPEATER
    #define USE_DCBLOCKER
    #define USE_ALTERNATE_POCSAG_LEDS

Compilar el código fuente:

    make pi-f722

Si estas usando Pi-Star, hay que detener el servicio MMDVMHost para liberar el puerto serial:

    sudo pistar-watchdog.service stop
    sudo systemctl stop mmdvmhost.timer
    sudo systemctl stop mmdvmhost.service

Sube el firmware a la tarjeta MMDVM:

    sudo make deploy-pi-f7

