---
title: Despliegue de páginas estáticas con Jekyll
date:  2020-07-23T00:00:51+00:00
layout: single
permalink: /post/despliegue-de-páginas-estáticas-con-jekyll
excerpt: "Siguiendo con el tema Jekyll y los blogs, entender la lógica de funcionamiento de Jekyll"
header:
    overlay_color: "#333"
    teaser: /assets/images/20/octojekyll.png
categories: 
    - CMS
    - Tech
tags:
    - blog
    - jekyllrb
    - cms
    - jekyll
---



# Despliegue de páginas estáticas con Jekyll

Siguiendo con el tema Jekyll y los blogs, este trabaja de manera diferente y si se entiende su manera de funcionar se vuelve mas sencillo, como dije la manera de trabajar de Jekyll es muy diferente a lo acostumbrado por que utiliza NO una base de datos y todos los archivos lo genera localmente. Preparé un gráfico para visualizarlo y entenderlo:

![Despliegue de Jekyll con Github](/assets/images/20/jekyll_despliegue.png) 

Una vez instalado Jekyll localmente:

1. Se crea el contenido localmente, se puede usar un editor de texto cualquiera  o un editor Markdown (Recomendable) siguiendo la guía del primer post de ejemplo. Se guarda el archivo generado en la carpeta `/posts/ `.

2. Con el comando `'jekyll serve'` Jekyll procesa el archivo creado y lo incluye junto a los demás documentos (de configuración y posts) para generar archivos estáticos [HTML] (y lo mete dentro de la la carpeta `/site`).

3. Tienes archivos generados dentro de tu carpeta raizl del blog, como dije Jekyl a partir de estos archivos genera un sitio plano dentro de la carpeta `/_site` y lo sirve localmente a través del puerto `4000`.

   En tu navegador digitando `http://localhost:4000` o `http://127.0.0.1:4000` tendrás desplegado tu sitio generado.

4. Aquí viene la magia de Jekyll y Git, especialmente con Github donde ya está instalado por defecto, lo único que se tiene que hacer es sincronizar en un repositorio creado donde previamente tienes que configurar tu `subdominio.github.io` También se puede usar otros repositorios que soporten Git y la lógica es la misma.

   ```bash
   git add .
   ```

   ```bash
   git commit -m "nombre del envío"
   ```

   ```bash
    git push origin master
   ```

   

   Con estos 3 comandos se logra sincronizar con Github. Estoy usando la consola de Linux, en Widows es mas fácil aun con Github para escritorio, es arrastrar y soltar.

   

5. Github ya tiene Jekyll instalado en sus servidores, por lo tanto al igual que localmente Github procesa los archivos sincronizados ("subidos").

6. Los archivos planos en [HTML] están listos para ser visualizados desde cualquier navegador del mundo.

7. Para hacerlo se tiene que ingresar a `htttps://tunombredeusuario.github.io`.

Este tema da para más por que luego se puede configurar un dominio personalizado e incluso un hosting especializado. Y porsupuesto añadir mas funciones como son los comentarios, temas, formularios, etc. Pero entendiendo lo básico se puede seguir.