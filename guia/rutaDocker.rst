Cambiar la ruta raiz de Docker CE
===================================

Cuando iniciamos docker toda la estructura es creada por defecto en  “/var/lib/docker”, pero la podemos cambiar, debemos crear el siguiente archivo daemon.json en el directorio /etc/docker.

Detenemos el servicio de docker::

	$ sudo ystemctl stop docker

Si estamos en initd debemos crear el daemon.json::

	$ vi /etc/docker/daemon.json
	{ 
	   "data-root": "/home/srv/docker" 
	}

Si estamos en systemd debemos modificar el docker.service.::

	$ vi /lib/systemd/system/docker.service
		# Buscar esta linea
		ExecStart=/usr/bin/dockerd 
		# Cambiar a:
		ExecStart=/usr/bin/dockerd -g /home/srv/docker


Recuerda luego copiar y/o mover el contenido de “/var/lib/docker” hacia la nueva ruta y ahora si podras iniciar el Docker en su nueva ruta.::

	# mv /var/lib/docker /home/srv/docker

Dememos recargar el systemctl::

	systemctl daemon-reload
	systemctl start docker

Ahora ejecutamos docker info tendremos la evidencia contundente::

	docker info | grep Root


Información del Docker instalado
++++++++++++++++++++++++++++++++

Consultar la info de docker::

	$ docker info
