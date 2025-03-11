# Creando fuentes RSS

## Introducción

A partir de un proyecto web de una tienda de regalos de ejemplos vamos a crear el fichero RSS de ejemplo que nos permita suscribirnos a la web y obtener los datos de los nuevos productos que añadamos a la tienda.

## Ramas

Para hacer el seguimiento de esta guía se van a crear una serie de ramas en el proyecto. Después de clonar el proyecto con:

```bash
$ git clone https://github.com/ichigar/lnd-rss-tienda.git
```

Podemos seguir los pasos del mismo cambiando a las ramas en las que se completa cada uno de los apartados con `git checkout nombre-rama`. Podemos ver las ramas del proyecto ejecutando `git branch`. Las ramas del proyecto son:

- `1-estructura-proyecto`
- `2-feed-proyecto`
- `3-facilitando-subscripcion`
- `4-validacion-rss`

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