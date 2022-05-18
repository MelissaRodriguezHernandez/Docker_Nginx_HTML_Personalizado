# Agregar HTML personalizado al servidor Nginx

> En esta práctica desplegaremos un HTML sobre el servidor web Nginx que a su vez esta ejecutandose en Docker

## Indíce

1. [Instalación Nginx en Docker](#1)
2. [Agregación de HTML personalizado](#2)
3. [Creación de imagen personalizada](#3)
4. [Resursos](#4)

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

![imagen wen nginx localhost](https://github.com/MelissaRodriguezHernandez/Docker_Nginx_HTML_Personalizado/blob/main/img/navegador%20nginx.png)

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

Primero crearemos nuestro directorio en local dentro de `Documentos` y le añadiremos el archivo con nuestro html

```
mkdir -p /nginx/site-content/
```

```
nano index.html
```
![contenido html personalizado](https://github.com/MelissaRodriguezHernandez/Docker_Nginx_HTML_Personalizado/blob/main/img/pagina%20personalizada.png)
 
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Docker Nginx</title>
</head>
<body>
  <h2>Melissa Rodriguez Hernandez</h2>
</body>
</html>
  ```

A continuación ejecutaremos el siguiente comando para crear un volumen y a la vez montar nuestro directorio local en el contener en ejecución de nginx (como ya hemos comentado antes)

```
docker run --rm -d -p 8080:80 --name web -v ~/Documentos/nginx/site-content:/usr/share/nginx/html nginx
  ```

Abrimos nuestro navegar en `localhost:8080`

![navegar página personalizada](https://github.com/MelissaRodriguezHernandez/Docker_Nginx_HTML_Personalizado/blob/main/img/navegador%20pagina.png)
  
</div>

<div id="3">
 
## Creación de imagen personalizada

Para crear nuestra imagen personalizada deberemos crear un `Dockerfile`, el cual crearemos en el directorio `./Documentos/nginx/`.
  
```
nano Dockerfile
  ```
Dentro de este archivo introduciremos el siguiente contenido

```
FROM nginx:latest
COPY ./site-content/index.html /usr/share/nginx/html/index.html
  ```
![imagen contenido Dockerfile](https://github.com/MelissaRodriguezHernandez/Docker_Nginx_HTML_Personalizado/blob/main/img/Dockerfile%20contenido.png)  

A continuación, copiamos nuestro archivo `index.html` situado en el directorio `./Documentos/nginx/site-content/` en el directorio `./usr/share/nginx/html/`.

```
cp index.html ./usr/share/nginx/html/
  ```
Para construir nuestra imagen deberemos ejecutar el siguiente comando

```
docker build -t webserver .
  ```
En la terminal debería salir algo así:
 
![terminal build server](https://github.com/MelissaRodriguezHernandez/Docker_Nginx_HTML_Personalizado/blob/main/img/build.png)

Finalmente iniciamos nuestro contenedor

![navegador contenedor](https://github.com/MelissaRodriguezHernandez/Docker_Nginx_HTML_Personalizado/blob/main/img/navegador%20contenedor.png)
  
</div>

<div id="4">
  
## Recursos

[Link]()
  </div>
