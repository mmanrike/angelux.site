---
title: Usando las dos tarjetas gráficas integradas con GNU/Linux
date:  2020-07-26T00:00:51+00:00
layout: single
permalink: /post/usando-dos-tarjetas-graficas-integradas-linux
excerpt: "Seguramente hemos vistos en las laptops que tienen tarjetas gráficas  integradas (Ya sean NVidia, Intel o AMD Radeón) como es en mi caso una modesta HP bw005la que viene  con una RADEON de 2 GBs, esta laptop viene sin SO instalado..."
header:
    overlay_color: "#333"
    teaser: /assets/images/20/tarjetasgraficas.jpg
categories: 
    - GNU/Linux
tags:
    - linux
    - configuración
    - gráficos
    - hardware en linux
---

# Usando las dos tarjetas gráficas integradas en tu laptop en GNU/Linux

Seguramente hemos vistos en las laptops que tienen tarjetas gráficas  integradas (Ya sean NVidia, Intel o AMD Radeón) como es en mi caso una modesta HP bw005la que viene  con una RADEON de 2 GBs, esta laptop viene sin SO instalado. Actualmente lo tengo con Ubuntu Mate exclusivo, realizando un análisis rápido de hardware ya sea por consola o por una aplicación gráfica nos muestra que en realidad existen 2 tarjetas y no una.

```bash
            .-/+oossssoo+/-.               angelux@AVM-HP 
        `:+ssssssssssssssssss+:`           -------------- 
      -+ssssssssssssssssssyyssss+-         OS: Ubuntu 20.04.1 LTS x86_64 
    .ossssssssssssssssssdMMMNysssso.       Host: HP Laptop 15-bw0xx 
   /ssssssssssshdmmNNmmyNMMMMhssssss/      Kernel: 5.4.0-7634-generic 
  +ssssssssshmydMMMMMMMNddddyssssssss+     Uptime: 2 mins 
 /sssssssshNMMMyhhyyyyhmNMMMNhssssssss/    Packages: 2183 (dpkg), 8 (snap) 
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Shell: bash 5.0.17 
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   Resolution: 1366x768 
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   DE: MATE 
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   WM: Metacity (Marco) 
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   WM Theme: OS-X-Yosemite-2.0 
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Theme: McOS-MJV-Gnome-3.30-1.1 [GTK2 
 /sssssssshNMMMyhhyyyyhdNMMMNhssssssss/    Icons: McMojave-circle [GTK2/3] 
  +sssssssssdmydMMMMMMMMddddyssssssss+     Terminal: mate-terminal 
   /ssssssssssshdmNNNNmyNMMMMhssssss/      Terminal Font: Ubuntu Mono 13 
    .ossssssssssssssssssdMMMNysssso.       CPU: AMD A9-9420 RADEON R5 2C+3G (2) 
      -+sssssssssssssssssyyyssss+-         GPU: AMD ATI Radeon HD 8670A/8670M/8 
        `:+ssssssssssssssssss+:`           GPU: AMD ATI Radeon R2/R3/R4/R5 Grap 
            .-/+oossssoo+/-.               Memory: 677MiB / 7423MiB 

                                                                   
                                                                   
```

* Nótese las 2 GPUs detectadas por el Neofetch.

La Primera tarjeta (Básica e Integrada), se usa en todo momento mientras no se exija un uso justificado, llámese juegos o despliegues de entornos 3D, videos FHD a más.

La segunda (Exclusiva y más potente), es la que se encarga de las tareas pesadas excluidas para la primera tarjeta.

Este tipo de configuración es la que se utiliza generalmente en todas las laptops de dichas características, sea con lo que sea la configuración entre las diversas marcas: AMD Radeón - AMD Radeón, Intel - AMD Radeón, Intel - Nvidia, etc.

En MS Windows los drivers propietarios incluyen una opción en su programa para que el traspaso entre ambas tarjetas sea casi automático e incluso hacerlo de manera manual.

En Gnu/Linux no me había dado cuenta de este detalle, de alguna manera ya no instalo juegos, pero haciendo el test de Hardware me entró la curiosidad de poder utilizar esa Tarjeta dedicada que tengo y la respuesta es que si se puede y es algo muy sencillo de realizarlo, eso si, debemos de hechar mano de la consola.

Asegurarse que las 2 tarjetas estén reconocidas por el sistema:

```bash
xrandr --listproviders
```

En mi caso me bota lo siguiente:

```bash
xrandr --listproviders
Providers: number : 2
Provider 0: id: 0x54 cap: 0x9, Source Output, Sink Offload crtcs: 2 outputs: 3 associated providers: 1 name:STONEY AMD Radeon GPU @ pci:0000:00:01.0
Provider 1: id: 0x7a cap: 0x4, Source Offload crtcs: 0 outputs: 0 associated providers: 1 name:HAINAN @ pci:0000:01:00.0

```

Como se puede observar  es la mismo info que arroja Neofetch, solo que de otra manera:

Tengo Provider 0: AMD Radeon GPU

y Provider 1: HAINAN.

También se puede saber esa información de manera gráfica con CPU-X:

![CPU-X Mostrando información de las Tarjetas Gráficas](/assets/images/20/graphics_hp_bw.png)

Una vez identificadas las tarjetas solo tenemos que añadir un Prefijo  antes de cualquier comando que se quiera ejecutar con la tarjeta dedicada `DRI_PRIME=1`:

Por ejemplo:

```bash
DRI_PRIME=1 glxinfo | grep "OpenGL renderer"
```

o para un juego en Wine dentro de la carpeta que contiene el juego o realizando la ruta larga, en mi caso suelo ocupar el Starcraft:

```bash
DRI_PRIME=1  wine broodwar.exe
```

También funciona con Steam y/o cualquier programa que se quiera ejecutar con la tarjeta dedicada. Los accesos directos se pueden editar y agregar el mismo sufijo.

Y listo, eso es todo. Hay más configuraciones pero eso es lo básico.

La lista completa e incluso otro programita que permite controlar con mas detalles lo encontré aquí:

[https://notebookgpu.blogspot.com/2018/05/configurar-graficos-hibridos-en-linux.html](https://notebookgpu.blogspot.com/2018/05/configurar-graficos-hibridos-en-linux.html)