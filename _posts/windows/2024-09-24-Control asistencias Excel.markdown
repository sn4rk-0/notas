---
layout: post
title: Control de Asistencia en Excel
date: 2024-09-24 11:47:23
category: Windows
vm: windows
---

> Generamos una tabla de control de asistencia usando emojis; metodo alternativo al uso de checkbox.

**1\.** Desarrollamos una tabla con nuestros estilos y campos requeridos.

**2\.** Seleccionamos los campos que marca las asistencias.

![](/notas/public/img/excel/exc1.png){:.margin}

**3\.** Click derecho y seleccionamos "Formato de celdas...".

![](/notas/public/img/excel/exc2.png){:.margin}

**4\.** Vamos a "Personalizada", y en "Tipo" eliminamos su contenido.

![](/notas/public/img/excel/exc3.png){:.margin}

**5\.** Agregamos la siguiente instruccion, en este punto necesitamos agregar emojis, presionamos `Windows + .` para abrir la ventana de emojis o copiamos de esta pagina <a href="https://emojiterra.com/" target="_blank"> Emojis</a>.

`[VERDE][=1]✔️;[ROJO][=0]❌`

![](/notas/public/img/excel/exc4.png){:.margin}

**6\.** Si podemos un 1 nos agrega el aspa, si ponemos un 0 nos marca la X

![](/notas/public/img/excel/exc5.png){:.margin}

**7\.** Añadimos la funcion suma de asistencias.

- En el campo que contendra la suma agregamos "=SUMA("
- Seleccionamos todos los campos de esa fila, se nos agrega las posiciones automaticamente en nuestra funcion "=SUMA(B2:G2)"
- Presionamos enter

![](/notas/public/img/excel/exc6.png){:.margin}

**8\.** Nos sale un formato incorrecto, en el apartado Numero podemos personalizar la salida.

![](/notas/public/img/excel/exc7.png){:.margin}

**9\.** Seleccionamos el formato Numero y le quitamos los valores decimales.

![](/notas/public/img/excel/exc8.png){:.margin}

**10\.** Por ultimo generamos la misma funcion para cada fila, arrastrando desde el cuadro de nuestra primera funcion hacia abajo.

![](/notas/public/img/excel/exc9.png){:.margin}

**11\.** Ya lo tenemos funcional el control de asistencia.

![](/notas/public/img/excel/exc10.png){:.margin}

<br>

<a href="#">_Finalizado._</a>