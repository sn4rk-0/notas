---
layout: post
title: Comandos básicos
date: 2024-03-28 16:28:23 +0900
category: sample
vm: linux
---
|Comando |Función|
|-|-|
|_**whoami**_ | muestra usuario del sistema|
|_**id**_ | ver los grupos del usuario|
|_**sudo su**_|ingresar como superusuario|
|_**whoami**_|  usuario del sistema|
|_**id**_|   identificador de grupos del usuario |
|_**sudo su**_|  escalar privilegio/acceder como root |
|_**exit**_|  salir|
|_**sudo** whoami_ | ejecutar comando como root|
|_**cat** archivo.txt_ | leer contenido de un archivo|
|_**which** cat_ | ver ruta absoluta de un comando|
|_**\|**_ |  volver a tratar la salida de un comando anterior|
|_**grep "filtro" -n**_ | filtrar y mostrar el numero de linea|
|_**comand -v** id_ | ver ruta absoluta de un comando|
|_**pwd**_ | muestra ruta en la que se esta posicionado|
|_**ls**_ | lista contenido en la posicion actual|
|_**ls -l**_ | lista contenido con mas detalles|
|_**tree -L 2**_ | lista contenido ramificado con **2** descenso|
|_**cd** Desktop_ | ingresar a un directorio|
|_**cd ..**_ | salir o retroceder un directorio|
|_**echo $PATH**_ | muestra ruta de binarios de los comandos|
|_**echo $HOME**_ | muestra ruta principal del usuario|
|`/etc/passwd` | ruta de archivos con los usuarios,id grupo,id usuario,...|
|_**echo $SHELL**_ | muestra tipo de shell (/etc/shells)|
|_id **;** whoami_ | ejecutar bloque de comandos|
|_id **&&** exit_ | ejecucion multiple, si el primero es exitoso se ejecuta el resto|
|_**echo $?**_ | codigo de estado de comando anterior ejecutado|
|_**2>/dev/null**_ | redirigir el stder o error|
|_**&>/dev/null**_ | redirigir el stder y stdout|
|_wireshark **&**_ | poner en segundo plano|
|_**& disown**_ | poner en segundo plano y desligar del proceso padre|
|_**exec** 3 **<>** file_ | descriptor de archivo creado con identificador **3**, **r** read,**w** write|
|_**>&** 3_ | agregar contenido al descriptor de archivo|
|_**exec** 3 **>&-**_ | cerrar un descriptor de archivo|
|_**exec** 5 **>&** 3_ | crea copia o descriptor al descriptor, ambos trabajan sobre el mismo archivo|
|_**touch** file.txt_ | crear archivo|
|_**file** file_ | muestra tipo de archivo|
|_**!$**_ | hace referencia al ultimo argumento ; (alt + .) nos pone el ultimo argumento|
|_**>**_ | sustituir contenido de un archivo|
|_**>>**_ | agregar contenido a un archivo|
|_**nano** file_ | editar archivo|
|`.rwx rwx rwx user group` | permisos (**.** archivo),(propietario),(grupos),(otros)|
|`drwx rwx rwx user group` | permisos (**d** directorio),(propietario),(grupos),(otros)|
|_**chmod o+w** file_ | cambiar permisos,**o**tros,**g**rupos,**u**suarios,**+** añadir,**-** quitar|
|_**mkdir** directory_ | crear directorio|
|_**rm -r** directory_ | eliminar recursivamente en caso de haber subcarpetas o archivos|
|_**chgrp** group file_ | cambiar grupo a un recurso |
|_**useradd** user **-s** /bin/bash **-d** /home/user_ | añadir usuario,**s** tipo shell, **d** directorio|
|_**passwd** user_ | asignar contraseña al usuario|
|_**chwon** user file_ | cambiar propietario a un recurso|
|_**chown** user:group file_ | cambiar propietario y grupo a un recurso|
|_**su** user_ | cambiar o migrar de usuario|
|_**groupadd** group_ | agregar o crear nuevo grupo|
|_**usermod -a -G** group user_ | añadir al grupo a un usuario|

_próxima actualización de contenido ...._