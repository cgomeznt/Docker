Crear una Imagen con Dockerfile
===============================

Vamos a crear una imagen muy simple con Dockerfile en este laboratorio.

Crear un directorio de trabajo
++++++++++++++++++++++++++++++
::

	$ mkdir laboratorio
	$ cd laboratorio/
	[oracle@nodo1 laboratorio]$

Crear el archivo Dockerfile
+++++++++++++++++++++++++++

Creamos un archivo Dockerfile con el siguiente contenido ::

	$ vi Dockerfile
	FROM centos:7

	MAINTAINER Carlos Gomez G cgomeznt@gmail.com

	# Declaramos las siguientes variables por recomendaciones d Docker
	ENV     container docker

	# Instalamos paquetes necesarios para la base que nos permitan administrar y hacer troubleshooting
	RUN     yum -y install sudo \
		unzip

	# Limpiamos los temporales de yum
	RUN     yum clean all

	# Creamos un usuario y grupo valido solo para ver que lo crea
	RUN     groupadd gprueba
	RUN     useradd -g gprueba uprueba

	# Creamos los directorios requeridos para copiar los archivos base, configuraciones y otras segun sea la necesidad. Tambien le otorgamos los permisos.
	RUN     mkdir -p /prueba/software && \
		mkdir -p /prueba1/EAR && \
		mkdir -p /prueba2

Creado la imagen con build
+++++++++++++++++++++++++++
::

	$ docker build -t "imagen-demostracion:1.0" .

El siguiente comando, si solo si, es en un Virtual Box y es para que tenga la salida de red por el host::

	$ docker build -t "imagen-demostracion:1.0" --network host .

Hacer un listado de las imagenes
+++++++++++++++++++++++++++++++++
::

	$ docker images

Crear el contenedor desde la imagen e iniciarlo
++++++++++++++++++++++++++++++++++++++++++++++++
::

	$ docker run -dti --name "contenedor-demostracion"  \
	-v /var/www/html:/var/www/html \
	--mount type=bind,source=/scm/external,target=/scm/external \
	--mount type=bind,source=/scm/EAR,target=/scm/EAR \
	-p 1234:80 \
	--privileged "imagen-demostracion:1.0" /usr/sbin/init

Estos argumentos "--privileged "imagen-demostracion:1.0" /usr/sbin/init" es para que funcione el systemctl 

ó::

	$ docker run -dti --name "contenedor-demostracion"  --mount type=bind,source=/home/qatest,target=/home/qatest -p 1234:80 "imagen-demostracion:1.0"

ó::

	$ docker run -dti --name "contenedor-demostracion"  -p 1234:80 "imagen-demostracion:1.0"

Ahora bien si estas en un Virtual box, agregale el --network host, claro el parametro -p queda deshabilitado. Tomara el que este expuesto en la imagen::

	$ docker run -dti --name "contenedor-demostracion"  \
	-v /var/www/html:/var/www/html \
	--mount type=bind,source=/scm/external,target=/scm/external \
	--mount type=bind,source=/scm/EAR,target=/scm/EAR \
	--network host \
	--privileged "imagen-demostracion:1.0" /usr/sbin/init


Consultar los contenedores que están iniciados.
+++++++++++++++++++++++++++++++++++++++++++++++
::

	$ docker ps

Ingresar al Contenedor en modo bash
+++++++++++++++++++++++++++++++++++
::

	$ docker exec -i -t contenedor-demostracion /bin/bash
	[oracle@ecde063fb19c /]$ 

Verificamos colocando en un navegador la URL administrativa del Weblogic.
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Listo podemos abrir un navegador y verificar que ya el Apache este operativo
http://nodo1:1234

Y si es en un virtual Box, recuerda que es el puerto por donde lo expones, en este caso sera el 80
http://nodo1

Ahora vamos a crear un archivo index para terminar con el laboratorio en el volumen persistente::

	$ sudo vi /var/www/html/index.html

	<html>
	  <head>
		<title>www.Docker-Demostracion.com</title>
	  </head>
	  <body>
		<h1>Felicitaciones, esta es un Apache dentro de un Contenedor Docker Demostracion</h1>
	  </body>
	</html>

Volvemos a consultar
http://nodo1:8080

Y si es en un virtual Box, recuerda que es el puerto por donde lo expones, en este caso sera el 80
http://nodo1

Listo ahora algunos comando de utilidad.

Detener el Contenedores
++++++++++++++++++++++++	
::

	$ docker stop contenedor-demostracion

Listar los Contenedores que no estan iniciados
++++++++++++++++++++++++++++++++++++++++++++++++
::

	$ docker ps -f "status=exited"

Iniciar el Contenedores
+++++++++++++++++++++++++++
::

	$ docker start contenedor-demostracion

Inspeccionar las configuraciones del Contenedores
+++++++++++++++++++++++++++++++++++++++++++++++++
::

	$  docker container inspect contenedor-demostracion

Borrar un Contenedores
++++++++++++++++++++++
::

	$ docker stop contenedor-demostracion && docker rm contenedor-demostracion

Borrar una Imagen
++++++++++++++++++++
::

	$ docker rmi fd40a4b4601f


Borrar Volumen huérfanos
+++++++++++++++++++++++++
::

	$ docker volume rm $(docker volume ls -qf dangling=true)





