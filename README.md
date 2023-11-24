# VSFTPD-Server
Configurar servidor VSFTPD

Aquí tienes una guía detallada para establecer la configuración de un servidor VSFTPD en un entorno UNIX (Mac). Es importante señalar que, aunque VSFTPD es comúnmente empleado en sistemas Linux, es posible instalarlo en un entorno Mac. Sin embargo, se podría contemplar la posibilidad de utilizar un servidor FTP diseñado específicamente para optimizar el rendimiento en macOS.

# Configuración de un Servidor VSFTPD en macOS

Nota: para poder arrancar esta práctica, siga los pasos de instalación de una máquina virtual en Azure en este enlace:
[Instalar-AzureVM](https://github.com/jousemarquez/Administracion-Servidores-Web)

## Paso 1: Instalar VSFTPD (en máquina virtual en Azure)

    sudo apt install vsftpd

![Brew](https://github.com/jousemarquez/VSFTPD-Server/blob/master/Screenshots/01.png?raw=true)<br>

## Paso 2: Comprobar Status

    sudo service vsftpd status

![Status](https://github.com/jousemarquez/VSFTPD-Server/blob/master/Screenshots/02.png?raw=true)<br>

## Paso 3: Modificar fichero vsftpd.conf

Editar el archivo de configuración:

    sudo nano /etc/vsftpd.conf

- Nota: asegurarse de tener configuraciones como estas:

    - write_enable=YES
    - local_enable=YES
    - chroot_local_user=YES
    - allow_writeable_chroot=YES

Y añadir las siguientes líneas de configuración:

  - allow_writeable_chroot=YES
  - secure_chroot_dir=/var/run/vsftpd/empty
  - pam_service_name=vsftpd
  - user_sub_token=$USER
  - local_root=/home/$USER/ftp
  - pasv_min_port=10000
  - pasv_max_port=10100

- Nota: usar ctrl + W para localizar cada línea más rápidamente.

Guarda y cierra el editor.


## Paso 4: Iniciar VSFTPD

    sudo brew services start vsftpd

![Launch](https://github.com/jousemarquez/VSFTPD-Server/blob/master/Screenshots/02.png?raw=true)<br>

## Paso 5: Comprobación del Estado del Servicio

    sudo brew services list

  - Nota: hay que asegurarse que vsftpd esté en estado started.

![Status](https://github.com/jousemarquez/VSFTPD-Server/blob/master/Screenshots/03.png?raw=true)<br>

## Paso 6: Añadir usuario

    sudo add user jouse

## Paso 7: Acceder al Servidor VSFTPD

Utiliza un cliente FTP, como FileZilla, para conectarte al servidor utilizando la dirección IP de tu Mac y el puerto 21.

Mi IP: 46.6.159.65

## Comandos Adicionales para Configuración Avanzada:

    sudo chown nobody:nogroup /home/username/ftp
    sudo chmod a-w /home/username/ftp
    sudo mkdir /home/username/ftp/upload
    sudo chown user:user /home/user/ftp/upload
    echo "My FTP Server" | sudo tee /home/user/ftp/upload/demo.txt

    user_sub_token=$USER
    local_root=/home/$USER/ftp

    userlist_enable=YES
    userlist_file=/etc/vsftpd.userlist
    userlist_deny=NO

    echo "user" | sudo tee -a /etc/vsftpd.userlist

    sudo openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem

    allow_anon_ssl=NO
    force_local_data_ssl=YES
    force_local_logins_ssl=YES
