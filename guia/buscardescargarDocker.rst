Buscar y descargar una imagen Docker
===================================

Buscar imágenes
++++++++++++++

Comience a buscar una imagen acoplable. En la lista, obtendremos el archivo de imagen oficial y más confiable en la primera línea.::

	# docker search nginx 

Descargar imágenes al servidor local
+++++++++++++++++++++++++

Para descargar con el archivo de imagen más reciente no es necesario mencionar la versión. Al descargar una imagen sin etiqueta, extraerá la última imagen que tiene "nginx:latest".

Siempre es bueno ir al sitio oficial y obtener más informcion de la imagen:

https://hub.docker.com/

De esta forma descargamos la ultima version dockerizada de nginx::

	# docker pull nginx

De esta otra forma podemos descargar un versión en especifico, pero siempre con la ayuda de https://hub.docker.com/

# docker pull nginx:1.14                  # Descargar la versión estable anterior sin etiqueta

Listar las imágenes descargadas.
++++++++++++++++++++++++++++

Para enumerar las imágenes locales::

	# docker images

Iniciar un contenedor desde imágenes docker
+++++++++++++++++++++++++++++++++

Instanciar la imagen descargada y se convertira en un contenedor::

	# docker run nginx

No deje de ver la documentacion en https://hub.docker.com/ para saber como se debe iniciar el contenedor y como se debe manipular.

Inicie un contenedor desde la imagen nginx: 1.14 en segundo plano. Para ejecutar un contenedor en segundo plano, debemos usar la opción “ -d ” modo separado.

Para asignar al servidor un nombre, usando la opción “ –name ” al iniciar un contenedor.::

#	 docker run -d --name nginx-test-server nginx

Para listar el contenedor en ejecución::

	# docker ps

Para detener el contenedor::

	# docker stop nginx-test-server

Para eliminar el contenedor::

	# docker rm nginx-test-server

Para eliminar la imagen en el servidor local::

	# docker rmi nginx
