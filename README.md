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

Al subir la nueva rama a GitHub se creará un PULL REQUEST para fusionar los cambios a la rama `main`. Accedemos a la web del repositorio en GitHub, aprobamos el PULL REQUEST.

Para que los cambios en `main` se sincronicen en local. Nos cambiamos a la rama `main` y descargamos los cambios con:

```bash
git checkout main
git pull
```

Si necesitamos crear una nueva rama ejecutamos:

```bash
git checkout -b nueva-rama
```
Y repetimos los pasos anteriores cada vez que terminemos con una rama.

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

## 3. Facilitando la subscripción

Con la configuración actual, para poder suscribirnos a la web debemos conocer la dirección exacta del fichero `rss.xml`. Para facilitar que los usuarios se puedan suscribir a nuestra web podemos hacerlo de varias maneras.

Una es incluir en el encabezado de la web una entrada que indique la ubicación del fichero de subscripción. Modificamos `head` en `index.html` para que quede de la forma:

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Regalos Mágicos</title>
    <link rel="alternate" type="application/rss+xml" title="Feed RSS - Tienda de regalos" href="feed/rss.xml">
    <link rel="stylesheet" href="css/styles.css">
</head>
```

Con lo anterior los buscadores de RSS y algunos clientes de RSS deberían ser capaces de encontrar la ubicación del fichero `rss.xml` de nuestro sitio simplemente poniendo la dirección de la web.

Otra forma de facilitar la suscripción es poner en alguna parte de la web un enlace directo al fichero `rss.xml`. Lo podemos hacer, por ejemplo incluyendo en el footer de la web un enlace directo al xml con su ubicación:

```html
<footer>
    <p>&copy; 2025 Regalos Mágicos</p>
    <div class="rss-subscribe">
        <a href="feed/rss.xml">Suscribirse al RSS</a>
    </div>
</footer>
```

Para mejorar la apariencia añadimos lo siguiente al fichero `styles.css`:

```css
.rss-subscribe {
    margin-top: 20px;
    margin-bottom: 10px;
}
.rss-subscribe a {
    display: inline-block;
    background-color: #f8b400;
    color: #fff;
    padding: 10px 15px;
    text-decoration: none;
    border-radius: 5px;
}
```

Los ficheros `index.html` y `styles.css` con las modificaciones los tienes en la rama `3-facilitando-subscripcion`

## 4. Validando el fichero rss.xml

Tal y cómo está definido el fichero rss.xml de nuestro sitio los clientes de RSS son capaces de leerlo y mostrar los artículos del mismo, pero si lo pasamos por herramientas de validación nos indicaran que le faltan elementos para considerarse válidos.

Podemos comprobar la validez de nuestro fichero RSS en la herramienta online <https://www.feedvalidator.org/> 


Para que `rss.xml` sea válido debemos añadir elementos de forma que quede:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
    <channel>
        <atom:link href="https://ichigar.neocities.org/tienda-regalos/feed/rss.xml" rel="self"
            type="application/rss+xml" />
        <title>Tienda de regalos mágicos</title>
        <link>https://ichigar.neocities.org/tienda-regalos/feed/rss.xml</link>
        <description>Últimos productos disponibles en nuestra tienda de regalos mágicos.</description>
        <item>
            <title>Caja de los Sueños</title>
            <link>https://ichigar.neocities.org/tienda-regalos/regalos/caja-de-los-suenos.html</link>
            <description>Una caja misteriosa llena de pequeños obsequios encantadores.</description>
            <guid isPermaLink="false">caja-de-los-suenos</guid>
            <pubDate>Fri, 28 Feb 2025 10:00:00 GMT</pubDate>
        </item>
        <item>
            <title>Reloj del Tiempo Eterno</title>
            <link>https://ichigar.neocities.org/tienda-regalos/regalos/reloj-tiempo-eterno.html</link>
            <category>Regalos</category>
            <description>Un elegante reloj con un diseño clásico y una historia fascinante.</description>
            <guid isPermaLink="false">reloj-tiempo-eterno</guid>
            <pubDate>Fri, 28 Feb 2025 09:30:00 GMT</pubDate>
        </item>
    </channel>
</rss>
```
## 5. Añadiendo productos a la web

Añade el siguiente producto al fichero index.html

```html
<article class="producto">
    <a href="regalos/peluche-suerte.html">
          <img src="images/peluche-suerte.jpg" alt="Peluche de la Suerte">
          <h3>Peluche de la Suerte</h3>
    </a>
</article>
```

Su página de detalle peluche-suerte.html:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Peluche de la Suerte - Regalos Mágicos</title>
    <link rel="stylesheet" href="../css/styles.css">
</head>
<body>
    <header>
        <h1>Regalos Mágicos</h1>
        <nav>
            <ul>
                <li><a href="../index.html">Inicio</a></li>
                <li><a href="../index.html#productos">Productos</a></li>
                <li><a href="../index.html#contacto">Contacto</a></li>
            </ul>
        </nav>
    </header>
    
    <main>
        <section class="producto-detalle">
            <img src="../images/peluche-suerte.jpg" alt="Peluche de la Suerte">
            <h2>Peluche de la Suerte</h2>
            <p><strong>Precio:</strong> $24.99</p>
            <p>El Peluche de la Suerte es un adorable compañero que trae fortuna y alegría a su dueño. Su suave textura y diseño encantador lo convierten en el regalo ideal para cualquier ocasión.</p>
            <p>"Abraza la buena suerte con este tierno peluche. Un compañero encantador que llenará tu vida de positividad y fortuna."</p>
            <a href="../index.html">Volver a la tienda</a>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2025 Regalos Mágicos</p>
        
    </footer>
</body>
</html>
```

Modificamos el fichero `rss.xml` para el nuevo producto añadiendo el nuevo `item`:

```xml
        <item>
            <title>Peluche de la suerte</title>
            <link>https://ichigar.neocities.org/tienda-regalos/regalos/peluche-suerte.html</link>
            <category>Regalos</category>
            <description>Un elegante reloj con un diseño clásico y una historia fascinante.</description>
            <guid isPermaLink="false">peluche-suerte</guid>
            <pubDate>Fri, 28 Feb 2025 09:30:00 GMT</pubDate>
        </item>
```

Al actualizar la suscripción en tu cliente de RSS debería aparecer el nuevo producto