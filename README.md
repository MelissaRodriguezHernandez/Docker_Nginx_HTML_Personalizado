# Agregar HTML personalizado al servidor Nginx

> En esta práctica desplegaremos un HTML sobre el servidor web Nginx que a su vez esta ejecutandose en Docker

Indíce

1. [Instalación Nginx en Docker](#1)
2. [Agregación de HTML personalizado](#2)

<div id="1">

## Instalación de Nginx en Docker
  
Para empezar buscaremos la imegen en DockerHub y la ejecutaremos para tener la imagen guardada en nuestro docker: [enlace](https://hub.docker.com/_/nginx)

```
pull nginx
```

A continuación ejecutaremos el contenedor en el puerto `8080` (localhost). Llamaremos a este contenedor `web`, esto se hace con la opción `--name`.
  
```
docker run --rm -d -p 8080:80 --name web nginx
  ```

Si abrimos nuestro navegador en el localhost deberia salir algo así:

![imagen wen nginx localhost]()

Esta sería la página por defecto del Nginx. Ahora ya tenemos las bases para continuar, y hacer una personalizada nuestra.
  
</div>

<div id="2">

## Agregación de HTML personalizado

Primero deberemos para el contenedor anterior

```
docker stop web
```

> Para continuar debemos tener en cuenta una serie de cosas. Por defecto, Nginx busca en el directorio `/usr/share/nginx/html` dentro del contenedor, por lo tanto necesitaremos necesitaremos tener nuestros archivos de la página que queremos mostrar en ese directorio.
Para lograr este objetivo deberemos montar un volúmen, para luego vincularlo a un directorio en nuestra máquina local y asignar ese mismo directorio al contenedor que se ejecutara.

Primero crearemos nuestro directorio en local y le añadiremos el archivo con nuestro html

```
mkdir -p /Documentos/nginx/site-content/
```

```
nano index.html
```
![contenido html personalizado]()

*el código html usado actual esta en la carpeta src de este repositorio*

A continuación ejecutaremos el siguiente comando para crear un volumen y a la vez montar nuestro directorio local en el contener en ejecución de nginx (como ya hemos comentado antes)

```
docker run --rm -d -p 8080:80 --name web -v ~/Documentos/nginx/site content:/usr/share/nginx/html nginx
  ```

  
</div>

