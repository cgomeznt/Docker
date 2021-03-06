Pasos actualizar ambientes
===========================

Existe el comando: 
	docker_update.sh <port>


Pero si quiere saber como hacerlo manual tienen lo siguiente:


Pasos para actualizar un ambiente en CONSIS Docker
++++++++++++++++++++++++++++++++++++++++++++++++++

Se ejecuta el procedimiento ya conocido para compilar.::

	[oracle@srvscm02 FE-CRECER-20180416]$ git-all.sh

	[oracle@srvscm02 FE-CRECER-20180416]$ cd scm/Make_EAR/*7054


	[oracle@srvscm02 Weblogic_CRECER_7054]$ ./make.sh 7054 s
	G-5543
	20180521 092154 Inicio de compilacion 
	COMPILANDO ACSELE
	Buildfile: /scm/workspace/CRECER/FE-CRECER-20180416/scm/Make_EAR/Weblogic_CRECER_7054/build.xml
	COMPILACION FINALIZADA

Consultamos cuales son los Containers que estan en ejecución.::

	[oracle@srvscm02 Weblogic_CRECER_7054]$ docker ps
	CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                              NAMES
	84bee0685c8b        weblogic:12.2.1.3.0   "/scm//scripts/start…"   2 days ago          Up 2 days           7022/tcp, 0.0.0.0:7054->7021/tcp   CRECER-7054
	1a5d26666303        weblogic:12.2.1.3.0   "/scm//scripts/start…"   2 days ago          Up 2 days           7022/tcp, 0.0.0.0:7022->7021/tcp   CRECER-7022

Detenemos el containers de CRECER-7054.::
	
	[oracle@srvscm02 Weblogic_CRECER_7054]$ docker stop CRECER-7054
	CRECER-7054

Volvemos a consultar cuales son los Containers que estan en ejecución y podemos ver que ya no esta CRECER-7054.::

	[oracle@srvscm02 Weblogic_CRECER_7054]$ docker ps
	CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                              NAMES
	1a5d26666303        weblogic:12.2.1.3.0   "/scm//scripts/start…"   2 days ago          Up 2 days           7022/tcp, 0.0.0.0:7022->7021/tcp   CRECER-7022

Necesitamos saber cual es el "VOLUMES" del container CRECER-7054.::

	[oracle@srvscm02 Weblogic_CRECER_7054]$ docker inspect CRECER-7054 | grep volumes
		        "Source": "/var/lib/docker/volumes/d45628fa01421a302888e1d133ea749de37d2c015cf85b3b61107a506f355d0b/_data",

Nos dirigimos al "VOLUMES" anteriormente indicado, pero agregamos la ruta del EAR.::

	[oracle@srvscm02 EAR]$ cd /var/lib/docker/volumes/d45628fa01421a302888e1d133ea749de37d2c015cf85b3b61107a506f355d0b/_data/EAR

Listamos el contenido de dicho directorio.::

	[oracle@srvscm02 EAR]$ ls
	CONSIS.ear 
	Respaldo

Realizamos un respaldo del EAR anterior.::

	[oracle@srvscm02 EAR]$ mv CONSIS.ear Respaldo/

Copiamos el nuevo EAR generado en el procedimiento de compilación.::

	[oracle@srvscm02 EAR]$ cp /scm/workspace/CRECER/FE-CRECER-20180416/scm/Make_EAR/Weblogic_CRECER_7054/dist/CONSIS.ear .

Iniciamos el containers de CRECER-7054.::

	[oracle@srvscm02 workspace]$ docker start CRECER-7054
	CRECER-7054

Consultamos cuales son los Containers que estan en ejecución y debemos ver al CRECER-7054.::

	[oracle@srvscm02 workspace]$ docker ps
	CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                              NAMES
	84bee0685c8b        weblogic:12.2.1.3.0   "/scm//scripts/start…"   2 days ago          Up 6 seconds        7022/tcp, 0.0.0.0:7054->7021/tcp   CRECER-7054
	1a5d26666303        weblogic:12.2.1.3.0   "/scm//scripts/start…"   2 days ago          Up 2 days           7022/tcp, 0.0.0.0:7022->7021/tcp   CRECER-7022

Vamos verificando el LOG de inicio del Weblogic.::

	[oracle@srvscm02 workspace]$ tail -f /var/lib/docker/volumes/d45628fa01421a302888e1d133ea749de37d2c015cf85b3b61107a506f355d0b/_data/logs/log_start.log 

Verificar la URL y certificar que tenga la nueva Revisión.

	http://srvscm02:7054/WController/





