---
title: Solución a tarjeta RTL8723DE en GNU/Linux con conexión lenta.
date:  2020-10-10T00:00:51+00:00
layout: single
permalink: /post/solucion-tarjeta-rtl8723de-linux-conexion-lenta
excerpt: "Tengo una portatil HP, no es la gran cosa pero corre  bien con GNU/Linux y lo tengo como único sistema, sin embargo su tarjeta de conexión wireless ha sido un dolor de cabeza para mi durante este tiempo, se trata del chip de Realtek RTL8723DE que presenta problemas con sus drivers para Linux."
header:
    overlay_color: "#333"
    teaser: /assets/images/20/rtl8723de.png
categories: 
    - linux
tags:
    - tarjeta
    - wireless
    - linux
    - rtl8723de
    - hp
---

# Solución a conexión inestable con tarjeta wireless RTL8723DE en Linux.

Tengo una portatil HP, no es la gran cosa pero corre  bien con GNU/Linux y lo tengo como único sistema, sin embargo su tarjeta de conexión wireless ha sido un dolor de cabeza para mi durante este tiempo, se trata del chip de Realtek RTL8723DE que presenta problemas con sus drivers para Linux.

## Parte 1: con kernel linux 4.*

Por defecto las distros de hace un par de años atrás no reconocía la tarjeta, por lo tanto había que echar mano a los drivers publicados gracias a la comunidad. Felizmente luego de instalado, la tarjeta iba muy bien, solo había que configurar la antena (1; 2 o 3). Sin embargo a medida que pasaba el tiempo las distros iban actualizando sus kernels (especialmente en las rolling release) y comenzaron a romperse los drivers, opté por una distro LTS (Xubuntu 18.04). Pero como todo distro hopper decidí dar una nueva oportunidad a la nueva LTS (Focal Fossa) pensando que ya había pasado el tiempo suficiente para que el nuevo kernel de la rama 5.* haya actualizado y mejorado la compatibilidad con mi tarjeta RTL8723DE, crazo error....

## Parte 2: con kernel linux 5.*

La nueva LTS de Ubuntu todas los sabores (uso XFCE) vienen con la rama del kernel 5.4.* Por defecto si reconoce la tarjeta RTL8723DE, sin embargo el dolor de cabeza viene cuando tienes tu señal incluso al 90% de pesadilla, ping con paquetes perdidos hasta el 100%, puede comenzar bien, pero mas temprano que tarde, la tarjeta se vuelve super inestable con la conexión (sin llegar a perder calidad de la señal), lo que me hizo pensar que podría tratarse de otro asunto, sin embargo nada funcionó.

Tocó a buscar el driver, gracias a la comunidad encontramos 2 métodos de instalarlo:

- Por [DKMS](https://github.com/smlinux/rtl8723de)
- Por [make y make install](https://github.com/lwfinger/rtw88)

Ambos drivers se encuentran en github y son muy sencillos de instalar siguiendo las instrucciones.

Incluso con los drivers instalados manualmente y cambiando la opción de selector de antenas tampoco la conexión iba muy mal, la señal seguía muy bien pero no funcionaba idoneamente.

Tocó probar con otras distros basados en Debian y Ubuntu. Resultado igual.

Pensé que con un kernel nuevo tal vez iría mejor, probé la familiar Arch (Manjaro y Endeveour) ambos con el kernel 5.8.* si reconocían la tarjeta, pero tampoco iba bien, super lento e intermitenta la conexión, incluso con la instalación manual de los drivers.

Probé en OpenSuse, con los mismos resultados, tocaba probar en Fedora 32 que también actualizando el kernel a 5.8.* tampoco.

Ya iba a echar la toalla, especialmente considerando que la anterior LTS (Ubuntu 18.04) ya vence en abril del próximo año, iba a regresar. Pero intuí que el kernel era el responsable, decidí probar nuevos kernels, como ya estaba en Fedora 32, busqué un repositorio que tenga kernels alternativos al oficial o poder contar con nuevos kernels seleccionados, pera eso en Ubuntu hay una utilidad excelente llamada [Ukuu](https://teejeetech.in/ukuu/), en Fedora tenemos uno que se llama [Kernel Vanilla](https://fedoraproject.org/wiki/Kernel_Vanilla_Repositories). Quienes empaquetan un kernel con diferentes configuraciones al usado en la versión principal y su función justamente es la de revisar errores y problemas que presental la rama principal.

## Solución: Usando Fedora 32 y un Kernel Vanilla

Configuré e instale un kernel vanilla stable.

1. Agregando los repositorios:

```bash
curl -s https://repos.fedorapeople.org/repos/thl/kernel-vanilla.repo | sudo tee /etc/yum.repos.d/kernel-vanilla.repo
```

2. Instalando  la última versión estable del kernel vanilla:

```bash
sudo dnf --enablerepo=kernel-vanilla-stable update
```

Reiniciar  e instalar el driver de la tarjeta wireless RTL8723DE de Git:

```bash
git clone https://github.com/lwfinger/rtw88.git
cd rtw88
make
sudo make install
```

Previamente necesitas instalar lo necesario para compilar:

```bash
sudo yum groupinstall "Development Tools" "Development Libraries"
```

Habilitamos el driver recién instalado:

```bash
sudo modprobe rtw_8723de            #This loads the module
```

Y finalmente probamos qué antena funciona mejor con el router:

```bash
sudo modprobe rtw_8723de ant_sel=1
```

Podemos seleccionar desde el 1 hasta el 3, a mi me funcionó correctamente con el 3.

Lo hacemos permanente:

```bash
sudo nano /etc/modprobe.d/rtw_8723de.conf
```

y añadimos:

```bash
options rtw_8723de ant_sel=3
```

Guardamos con Ctrl + O y Cerramos con Ctrl + X.

Reiniciamos y ya debería estar. En mi caso Fedora lo usé anteriormente y me fue de maravilla, se puede configurar e instalar de acuerdo a tus necesidades, si necesitas usar GNU/Linux, no pierdes nada probando Fedora, es una gran distro también a cambio tienes la tarjeta estable sin necesidad de comprar una nueva.

En caso quieras usar una distro debianita, sería cuestión de probar kernels alternativos, en la familia *Ubuntu, Ukuu es la herramienta a probar.