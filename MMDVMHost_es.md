# Instalación de MMDVMHost como servicio

Si no tiene instalado MMDVMHost, bajar desde esta ubicación:

    cd ~
    git clone https://github.com/g4klx/MMDVMHost

Compile e instale MMDVMHost:

    cd ~/MMDVMHost/
    make
    sudo cp MMDVMHost /usr/local/bin/
    sudo cp MMDVM.ini /etc/
    sudo cp DMRIds.dat /etc/
    sudo cp RSSI.dat /etc/

Ingrese a la siguiente ubicación dentro de su instalación de MMDVMHost:

    cd ~/MMDVMHost/linux/init

Copiar el archivo de servicio a la siguiente ubicación:

    sudo cp mmdvmhost /etc/init.d/

Editar este archivo:

    sudo nano /etc/init.d/mmdvmhost

y cambiar:

    CONFIG=/etc/MMDVM.ini
    USER=root
    GROUP=root
    LOGDIR=/var/log/

Cambiar permisos de este archivo:

    sudo chmod 550 /etc/init.d/mmdvmhost
    sudo chown root:root /etc/init.d/mmdvmhost

Habilitar servicio:

    sudo update-rc.d mmdvmhost defaults

Editar MMDVM.ini:

    sudo nano /etc/MMDVM.ini

y editar de acuerdo a tus requerimientos, pero además cambiar:

    [Log]
    FilePath=/var/log/
    
    [DMR Id Lookup]
    File=/etc/DMRIds.dat
    
    [Modem]
    RSSIMappingFile=/etc/RSSI.dat

Una vez hechos estos cambios, iniciar el servicio:

    sudo service mmdvmhost start

Para ver el log de MMDVMHost, usar:

    tail -f /var/log/MMDVM-YYYY-MM-DD.log

donde YYYY-MM-DD depende de la fecha actual. Desde ahora en adelante el servicio mmdvmhost debería iniciar automáticamente con el arranque del sistema.

# Agregar actualización automática de DMRIds.dat

Copiar script:

    cd ~/MMDVMHost/linux/
    sudo cp DMRIDUpdateBM.sh /etc/
    sudo chmod +x /etc/DMRIDUpdateBM.sh

Editar DMRIDUpdateBM.sh:

    sudo nano /etc/DMRIDUpdateBM.sh

y cambiar:

    DMRIDFILE=/etc/DMRIds.dat
    RESTARTCOMMAND="service mmdvmhost restart"

Agregar el script al cron. Para ello editar el siguiente archivo:

    sudo nano /etc/crontab

y agregar la siguiente linea al final:

    0  4  *  *  *  /etc/DMRIDUpdateBM.sh 1>/dev/null 2>&1

Esto último actualiza automáticamente DMRIds.dat a las 4:00 am cada día.
