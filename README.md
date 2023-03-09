# Contenedor Docker+Python

## Creación del script de python

Es necesarioa la instalación de un paquete de python llamado `pytube`, esto se consigue con el comando `pip install pytube`.
Sin este paquete no se podrá hacer el `import` de las librerias necesarias para la descarga y manipulación de los videos. 

En el siguiente script la variable **yt** guardará el link al video que se quiere manipular.
En los prints se ve que se pone ***yt*** con la extension `views, leght, description, rating` 
estos sacan la información que la propia extensión indica.
```
from pytube import YouTube

yt = YouTube("https://www.youtube.com/watch?v=halHIc6yPJ4")

#Titulo
print("Title: ",yt.title)
#Numero de views
print("Number of views: ",yt.views)
#Lonjitud del video
print("Length of video: ",yt.length,"seconds")
#Descripción del video
print("Description: ",yt.description)
#Rating del video
print("Ratings: ",yt.rating)

```

## Creación del archivo Docker

La **DockerFile** es necesaria para crear una imagen por lo que para crearla:
```
touch Dockerfile requirements.txt
chmod 777 Dockerfile requirements.txt
```
El fichero **Dockerfile** debe contener lo siguiente:
```
FROM python:3

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN pip install --no-cache-dir -r requirements.txt
```
El fichero **requirements.txt** debe contener lo siguiente:
```
pytube=12.1.2
```
Una vez todos estos archivos están creados y con los permisos necesarios ejecuto el comando `docker build .`
para poder montar la imágen como se pide en el enunciado.

## Docker-compose

Dentro de la carpeta en la que se está trabajando se debe crear un archivo llamado `docker-compose.yml` y que definirá las características del contenedor o contenedores que se quieran lanzar.
El contenido del docker-compose debe ser el siguiente:
```
services:
  python:
    image: youtubeimage:chromecast
    volumes:
      - ./app:/usr/src/app
    stdin_open: true
    tty: true
    working_dir: /usr/src/app
    command: python3
```

## Montaje de la imágen

Al igual que antes para hacer la imágen ejecuto `docker build` sobre el dockerfile creado, de esta manera, la sintaxis requerida esta vez es la siguiente: `docker build -t nombreimagen:version .`
Este comando hace uso del *dockerfile* por lo que de ser necesario hay que indicar la ruta absoluta o relativa en caso de no encontrarse en la misma carpeta donde se está trabajando.

Ahora, ejecuto `docker run` del contenedor de la siguiente manera: `docker run python3`

Al ejecutar el contenedor el video del link del script de python se debería descargar de manera inmediata.

## Imagen personalizada

https://hub.docker.com/layers/damiansigueiras/youtubeimagen2/latest/images/sha256-7d13328718fcba6a91e25ee64d2485af3eaa8fee30667f4b535a48479bb7c748?context=repo

