# Creando fuentes RSS

## Introducción

A partir de un proyecto web de una tienda de regalos de ejemplos vamos a crear el fichero RSS de ejemplo que nos permita suscribirnos a la web y obtener los datos de los nuevos productos que añadamos a la tienda.

## Ramas

Para hacer el seguimiento de esta guía se van a crear una serie de ramas en el proyecto. Después de clonar el proyecto con:

```bash
git clone https://github.com/ichigar/lnd-rss-tienda.git
```

Podemos seguir los pasos del mismo cambiando a las ramas en las que se completa cada uno de los apartados con `git checkout nombre-rama`. Podemos ver las ramas del proyecto ejecutando `git branch`. Las ramas del proyecto son:

- `1-estructura-proyecto`
- `2-feed-proyecto`
- `3-facilitando-subscripcion`
- `4-validacion-rss`
- `5-nuevos-productos`
- `6-github-pages`

### Trabajando con las ramas en git y GitHub

De forma genérica vamos a trabajar de la siguiente forma una vez creado y clonado el repositorio de GitHub:

Sincronizamos con el repositorio remoto, creamos nueva rama y entramos en ella:

```bash
git pull
git checkout -b nombre-rama
```

Hacemos modificaciones de archivos en la rama, los añadimos a la rama, hacemos commit y los subimos al repositorio remoto en Github:

```bash
git add .
git commit -m "Descripción de modificaciones"
git push origin nombre-rama
```

Una vez terminamos con una rama fusionamos los cambios a la rama `main` y creamos una nueva rama para el paso siguiente:

```bash
git checkout main
git merge nombre-rama
git checkout -b nueva-rama
```

## 1. Estructura del proyecto

Partimos de la siguiente estructura de archivos para nuestro proyecto:

```bash
/tienda_regalos
├── css
│   └── styles.css
├── images
│   ├── caja-suenos.jpg
│   └── reloj-eterno.jpg
├── index.html
└── regalos
    ├── caja-de-los-suenos.html
    └── reloj-tiempo-eterno.html
```

La web de prueba estará alojada en `neocities.org` la URL base de la misma sera <https://ichigar.neocities.org/tienda-regalos>

## 2. Feed del proyecto

Queremos que los usuarios de la web se puedan suscribir por RSS a la misma de forma que estén informados de los nuevos productos que se añadan a la tienda. Un fichero básico de RSS tiene la siguiente estructura:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0">
  <channel>
    <title>Ejemplo de Feed RSS</title>
    <link>https://www.ejemplo.com</link>
    <description>Últimas noticias y actualizaciones</description>
    
    <item>
      <title>Primera noticia</title>
      <link>https://www.ejemplo.com/noticia1</link>
      <description>Descripción breve de la noticia</description>
      <pubDate>Fri, 28 Feb 2025 12:00:00 GMT</pubDate>
    </item>

    <item>
      <title>Segunda noticia</title>
      <link>https://www.ejemplo.com/noticia2</link>
      <description>Descripción breve de la segunda noticia</description>
      <pubDate>Fri, 28 Feb 2025 14:00:00 GMT</pubDate>
    </item>

  </channel>
</rss>
```

Siguiendo la estructura anterior añadimos a la estructura de nuestro proyecto la subcarpeta `feed` en la que se alojará el fichero `rss.xml` con los datos de la web y de los dos productos actuales de la misma:

```bash
/tienda_regalos
├── css
│   └── styles.css
├── feed
│   ├── rss.xml
├── images
│   ├── caja-suenos.jpg
│   └── reloj-eterno.jpg
├── index.html
└── regalos
    ├── caja-de-los-suenos.html
    └── reloj-tiempo-eterno.html
```

Teniendo en cuenta que la URL inicial en la que se aloja el proyecto es <https://ichigar.neocities.org/tienda-regalos> el fichero `rss.xml` tendrá el siguiente contenido:

```xml
?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
    <channel>
        <title>Tienda de regalos mágicos</title>
        <link>https://ichigar.neocities.org/tienda-regalos/feed/rss.xml</link>
        <description>Últimos productos disponibles en nuestra tienda de regalos mágicos.</description>
        
        <item>
            <title>Caja de los Sueños</title>
            <link>https://ichigar.neocities.org/tienda-regalos/regalos/caja-de-los-suenos.html</link>
            <description>Una caja misteriosa llena de pequeños obsequios encantadores.</description>
            <pubDate>Fri, 28 Feb 2025 10:00:00 GMT</pubDate>
        </item>

        <item>
            <title>Reloj del Tiempo Eterno</title>
            <link>https://ichigar.neocities.org/tienda-regalos/regalos/reloj-tiempo-eterno.html</link>
            <description>Un elegante reloj con un diseño clásico y una historia fascinante.</description>
            <pubDate>Fri, 28 Feb 2025 09:30:00 GMT</pubDate>
        </item>
    </channel>
</rss>
```

Una vez añadido el fichero anterior nos podremos suscribir a las novedades de productos de la tienda desde cualquier cliente de RSS introduciendo la dirección: <https://ichigar.neocities.org/tienda-regalos/feed/rss.xml>