Docker export / import / save / load
===================================

save funciona con imágenes de Docker. Guarda todo lo necesario para construir un contenedor desde cero. Utilice este comando si desea compartir una imagen con otras personas.

load funciona con imágenes de Docker. Utilice este comando si desea ejecutar una imagen exportada con save. A diferencia de pull, que requiere conectarse a un registro de Docker, la carga se puede importar desde cualquier lugar (por ejemplo, un sistema de archivos, URL).

export funciona con contenedores Docker y exporta el filesystem de un contenedor a un archivo.

import funciona con el sistema de archivos de un contenedor exportado y lo importa como una imagen de Docker. Use este comando si tiene un sistema de archivos exportado que desea explorar o usar como capa para una nueva imagen.

::

	$ docker --help | grep -E "(export|import|load|save)"

::

	$ cat Dockerfile
	FROM busybox

	MAINTAINER Carlos Gomez G cgomeznt@gmail.com

	CMD echo $((7*3))

::

	$ docker build -t "simple-app:1.0" .

::

	$ docker run --name simple-app1.0 simple-app:1.0


Salvamos la imagen con save en un archivo::

	$ docker save simple-app:1.0 > simple-app1.0.tar

Cargamos la imagen almacenada en un archivo con load::

	$ docker load < simple-app1.0.tar




