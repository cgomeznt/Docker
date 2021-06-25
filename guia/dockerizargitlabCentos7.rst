Dokerizar Gitlab Gitlab-Runner Centos 7
===============================

Vamos a Dokerizar Gitlab, Gitlab-Runner paso a paso en este docker.

Recordemos deshabilitar el selinux y firewall, importante tener una buena configuración de DNS o en el archivo HOSTS

**Descargamos una imagen de Centos 7**::

	docker search centos

	docker pull centos

	docker images 

**Crear un directorio de trabajo**::

	$ mkdir laboratorio

**Creamos un Network en docker**::

	docker network create app

**Creamos la variable para un volumen permanente**::

	export GITLAB_HOME="/home/cgomeznt/srv/gitlab"

**Instanciamos el contenedor**::

	docker run -dti \
	--hostname gitlab.dominio.local \
	--publish 443:443 \
	--publish 80:80 \
	--publish 22:22 \
	--name gitlab \
	--restart=on-failure \
	--network app \
	--volume $GITLAB_HOME/config:/etc/gitlab \
	--volume $GITLAB_HOME/logs:/var/log/gitlab \
	--volume $GITLAB_HOME/data:/var/opt/gitlab --privileged centos:7 /usr/sbin/init

**Descargar el gitlab gitlab-runner**

https://about.gitlab.com/install/

https://docs.gitlab.com/runner/install/linux-manually.html

**Copiamos el instalador gitlab gitlab-runner**::

	docker cp gitlab-ce-13.10.0-ce.0.el7.x86_64.rpm gitlab/tmp
	docker cp gitlab-runner_amd64.rpm gitlab/tmp

**Ingresamos en le contenedor**::

	docker exec -ti gitlab bash

**Instalamos los pre-requisitos**::

	yum install -y xterm curl openssh-server ca-certificates postfix policycoreutils-python vim net-tools git

**Instalamos gitlab**::

	rpm -ivh /tmp/gitlab-ce-13.10.0-ce.0.el7.x86_64.rpm

**Modificamos el worker_timeout** para evitar que nos errores por timeout, error 502::

	sed -i "s/\# unicorn\['worker_timeout'\] = 60/unicorn\['worker_timeout'\] = 300/g" /etc/gitlab/gitlab.rb

**Editamos la URL**::

	vi /etc/gitlab/gitlab.rb
	...
	# external_url 'http://gitlab.example.com'
	external_url 'http://gitlab.dominio.local'
	...

**Iniciamos el servicio de gitlab** Esto lo debes ejecutar cada vez que inicies el contenedor, **NOTA**  en este link oficial explica porque esta configuración, si no la aplicamos el Gitlab se quedara en la reconfiguración pegado en esta sesión *** ruby_block[wait for redis service socket] action run**

https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/4257 ::


(/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure

Listo, con esto ya podemos cargar la pagina de gitlab y cambiar la clave de root

http://gitlab.dominio.local

**Instalamos Docker**

https://docs.docker.com/engine/install/centos/ ::

	yum install -y yum-utils

	yum-config-manager \
	    --add-repo \
	    https://download.docker.com/linux/centos/docker-ce.repo

**Iniciamos docker**::

	systemctl enable docker
	systemctl start docker
	systemctl status docker


**Instalar gitlab-runner**

https://docs.gitlab.com/runner/install/linux-manually.html ::

	rpm -ivh /tmp/gitlab-runner_amd64.rpm

**El usuario gitlab-runner debe estar en el grupo Docker**::

	usermod -aG docker gitlab-runner
	newgrp docker
	id gitlab-runner

**Instalamos una versión superior de git** porque el git 1.8.3.1 No soporta git fetch-pack

https://stackoverflow.com/questions/56663096/gitlab-runner-doesnt-work-on-a-specific-project ::

	git --version
	git version 1.8.3.1 # No soporta git fetch-pack

	yum -y install https://packages.endpoint.com/rhel/7/os/x86_64/endpoint-repo-1.7-1.x86_64.rpm
	yum install git
	git --version


**Registramos un runner dentro del gitlab**::

	gitlab-runner register









