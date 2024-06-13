---
layout: post
title: "Papa frita"
date: 2024-06-11 16:25:23
category: sample
vm: hackerlabs
---

<style>
  .post-content {
    color: #51c25be1; /* Cambia el color del texto */
  }
</style>

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/thehackerlabs/thehackerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Papa frita </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Dificultad:</strong> <span class="easy">Facil</span></td> </tr>
  <tr> <td><strong><a href="https://thehackerslabs.com/papafrita/" target="_blank"> Descarga</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/thehackerlabs/Papafrita/host.png){:.margin}

**1\.** Escaneo la red en busca de host activos.

`sudo arp-scan -l`

![img](/notas/public/img/thehackerlabs/Papafrita/arp.png){:.margin}

**2\.** Escaneo los puertos activos y servicios que corre la maquina. 

`sudo nmap -p- --open -T5 -n -Pn -sCV --min-rate=5000 192.168.1.9`

![img](/notas/public/img/thehackerlabs/Papafrita/nmap.png){:.margin}

**3\.** En la web corre la pagina Apache por defecto, pero en el codigo hallo un texto en Brainfuck.

`http://192.168.1.9`

![img](/notas/public/img/thehackerlabs/Papafrita/80.png){:.margin}
![img](/notas/public/img/thehackerlabs/Papafrita/80code.png){:.margin}

**4\.** Lo decodeo con  [dcodefr](https://www.dcode.fr/brainfuck-language) , puede ser una posible contraseña.

![img](/notas/public/img/thehackerlabs/Papafrita/brainfuck.png){:.margin}

**5\.** Wua en este punto no encuentro nada, la unica fue dejar el hydra un buen rato, al fin dio con la pass.

`hydra -L /usr/share/wordlists/rockyou.txt -p abuelacalientalasopa ssh://192.168.1.9`

![img](/notas/public/img/thehackerlabs/Papafrita/hydra.png){:.margin}

**6\.** Me conecto por ssh.

`ssh abuela@192.168.1.9`

![img](/notas/public/img/thehackerlabs/Papafrita/ssh.png){:.margin}

**7\.** Escalando privilegios.

_7.1- Encuentro un binario que puedo ejecutarlo como root_

`sudo -l`

![img](/notas/public/img/thehackerlabs/Papafrita/sudol.png){:.margin}

_7.2- Busco la forma de escalar en [gtfobins](https://gtfobins.github.io/gtfobins/node/#sudo)_

![img](/notas/public/img/thehackerlabs/Papafrita/root.png){:.margin}

<br>

<span class="finish">_Finalizado._</span>