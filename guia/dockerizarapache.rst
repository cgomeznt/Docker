Dokerizar Apache paso a paso
===============================

Vamos a Dokerizar Apache paso a paso en este laboratorio.

Hacer una imagen de Docker CE con Dockerfile
++++++++++++++++++++++++++++++++++++++++++++

Vamos a crear una imagen con la ayuda del archivo Dockerfile, dentro de él vamos a colocar todas las lineas de instrucciones necesarias para que se descargue una imagen base, luego dentro de ella vamos a copiar algun archivo y/o instaladores que necesitemos para hacer este laboratorio y por ultimo con el comando build procedemos a crear la imagen.

Crear un directorio de trabajo
++++++++++++++++++++++++++++++
::

	$ mkdir laboratorio
	$ cd laboratorio/
	[oracle@nodo1 laboratorio]$

Crear el archivo Dockerfile
+++++++++++++++++++++++++++

Vamos a crear un Dockerfile que haga lo siguiente.:

Crear una base de la imagen.

Actualizar la base de la imagen.

Crear el usuario y grupo.

Crear los directorios para demostración.

Crear las variables para demostración.

Crear variable de entorno para demostración.

Copiar los archivos base y de configuración dentro de la imagen.

Asignar los permisos a los directorios creados.

Instalar el Apache.

Instalar la versión de JAVA.

Cambiarse con el usuario creado realizar la instalación.

Iniciar el servicio de Apache con systemctl. Utilizar systemctl tiene unos tips

Crear un volumen que permite modificar, eliminar o agregar archivos y/o directorios luego que el CONTENEDOR este en uso.

Exponer el puerto por donde una aplicación escuchara las peticiones. para demostración.

Así quedaría el archivo Dockerfile y lo copiamos en en directorio laboratorio::

	$ vi Dockerfile
	FROM centos:7

	MAINTAINER Carlos Gomez G cgomeznt@gmail.com

	# Declaramos las siguientes variables por recomendaciones d Docker
	ENV     container docker

	# Instalamos paquetes necesarios para la base que nos permitan administrar y hacer troubleshooting
	RUN     yum -y install sudo \
		httpd.x86_64 \
		net-tools \
		unzip

	# Limpiamos los temporales de yum
	RUN     yum clean all

	# Creamos el usuario y grupo valido para inicializar el Weblogic
	RUN     groupadd oinstall
	RUN     useradd -g oinstall oracle

	# Creamos los directorios requeridos para copiar los archivos base, configuraciones y otras segun sea la necesidad. Tambien le otorgamos los permisos.
	RUN     mkdir -p /u01/software && \
		mkdir -p /scm/EAR && \
		mkdir -p /scm/external && \
		mkdir -p /scm/scripts && \
		mkdir -p /scm/logs && \
		mkdir -p /u01/app/oracle/middleware && \
		mkdir -p /u01/app/oracle/config/domains && \
		mkdir -p /u01/app/oracle/config/applications

	# Creamos las variables para demostración
	ENV     export MW_HOME=/u01/app/oracle/middleware
	ENV     export WLS_HOME=$MW_HOME/wlserver
	ENV     export WL_HOME=$WLS_HOME

	# Creamos la variable del JAVA_HOME y lo colocamos en el PATH
	ENV     export JAVA_HOME=/u01/app/oracle/jdk1.8.0_77
	ENV     export PATH=$JAVA_HOME/bin:$PATH

	# Copiamos la version del JAVA y lo instalamos
	COPY    jdk-8u101-linux-x64.rpm /u01/software
	RUN     rpm -ivh /u01/software/jdk-8u101-linux-x64.rpm

	# Copiamos los archivos base y de configuracion dentro de la imagen.
	COPY    startMyAPP.sh /scm/scripts
	COPY    stopMyAPP.sh /scm/scripts
	RUN     chown -R oracle:oinstall /u01 && \
		chown -R oracle:oinstall /scm && \
		chmod -R 775 /u01/ && \
		chmod -R 775 /scm/

	# Limpiamos todos los archivo que ya no son requeridos para la imagen.
	RUN     rm -rf /u01/software/*

	# Creamos este volumen que nos permite modificar, eliminar o agregar archivos y/o directorios luego que el CONTENEDOR este en uso.
	VOLUME  "/scm"

	RUN	systemctl enable httpd

	# Cuando el CONTENEDOR este operativo, el host expondra este puerto.
	ARG     PORT=80
	EXPOSE  $PORT

	#Lanzar la ejecución de una aplicacion.
	#CMD ["/u01/app/oracle/middleware/user_projects/domains/D7021/bin/startWebLogic.sh"]
	#CMD ["/scm/scripts/startMyAPP.sh"]
	RUN	/scm/scripts/startMyAPP.sh


Copiar los instaladores necesarios y los archivos de configuración que serán utilizados desde el archivo Dockerfile, en nuestra carpeta de trabajo::

	[oracle@nodo1 laboratorio]$ ls
	jdk-8u101-linux-x64.rpm  startMyAPP.sh  stopMyAPP.sh

Paso a paso de la creación de la imagen y del contenedor.
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Ya que tenemos cuales archivos vamos a utilizar vamos a continuar con un paso a paso técnico.

Nos aseguramos que estamos en el directorio de trabajo y que están todos los archivos requeridos.
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
::

	[oracle@nodo1 laboratorio]$ pwd
	[oracle@nodo1 laboratorio]$ ls
		Dockerfile  jdk-8u101-linux-x64.rpm  startMyAPP.sh  stopMyAPP.sh

Creado la imagen con build
+++++++++++++++++++++++++++
::

	[oracle@nodo1 laboratorio]$ docker build -t "imagen-demostracion:1.0" --build-arg PORT=80 .

El siguiente comando, si solo si, es en un Virtual Box y es para que tenga la salida de red por el host::

	[oracle@nodo1 laboratorio]$ docker build -t "imagen-demostracion:1.0" --build-arg PORT=80 --network host .

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





