## Docker

* [¿Qué es Docker? - Primeros Pasos](guia/QueesDocker.rst)
* [Instalar Docker](guia/instalarDocker.rst)
* [Administrar Docker como un usuario no root](guia/usuarionoroot.rst)
* [Cambiar la ruta de Docker](guia/rutaDocker.rst)
* [Buscar y descargar una imagen Docker](guia/buscardescargarDocker.rst)
* [Que es Dockerfile](guia/QueesDockerfile.rst)
* [Crear una Imagen con Dockerfile](guia/Dockerfilesimple.rst)
* [Guardar los cambios del contenedor en una nueva imagen](guia/cambioscontenedornuevaimagen.rst)
* [Comandos más utilizados](guia/comandosmasutilizados.rst)
* [Dokerizar Apache paso a paso](guia/dockerizarapache.rst)
* [Dokerizar Gitlab paso a paso](guia/dockerizargitlab.rst)
* [Step-step con Dockerfile](guia/Step_step_con_Dockerfile.rst)
* [Step-step con Dockerfile para Weblogic](guia/Step_step_con_Dockerfile_para_Weblogic.rst)
* [Export and Import a Docker Image Between Nodes](guia/Export_and_Import_a_Docker_Image_Between_Nodes.rst)
* [Pasos Actualizar Ambiente](guia/Pasos_Actualizar_Ambiente.rst)
* [Docker paso a paso para CONSIS con WebLogic](guia/Docker_paso_paso_CONSIS_WebLogic.rst)
* [Docker paso a paso para CONSIS con WAS](guia/Docker_paso_paso_CONSIS_WAS.rst)
* [Un buen build run exec para CONSIS](guia/build-run-exec.rst)

Crear y mantener imágenes de contenedores de Docker
Publicar imágenes de contenedores personalizados en un repositorio privado
Implementar aplicaciones de varios niveles con la ayuda de contenedores
Utilizar Docker para una aplicación existente

[cgomez@lcsprdapptdldap02 ~]$ sudo cat /etc/cron.d/gestionlogs
#@midnight root find /var/lib/ldap/*.gz -exec rm {} \; 1> /dev/null 2> /dev/null
#@midnight root find /var/lib/ldap/log.* -mtime +3 -exec rm {} \; 1> /dev/null 2> /dev/null
@midnight root /usr/local/bin/depurar_memoria.sh


ldapsearch -xLLL -b "dc=credicard,dc=com,dc=ve" mail=lucybell.sanchez@gmail.com  sn givenName cn
ldapsearch -x -b dc=credicard,dc=com,dc=ve mail=* dn | grep dn: | wc -l

ldapsearch -x -LLL -s base -b 'dc=credicard,dc=com,dc=ve' contextCSN dn: dc=credicard,dc=com,dc=ve
ldapsearch -x -LLL -H ldap://slave:389 -s base -b 'dc=credicard,dc=com,dc=ve' contextCSN dn: dc=credicard,dc=com,dc=ve

slapd.conf
checkpoint 1024 10
cachesize 10000
overlay syncprov
syncprov-checkpoint 1 1
syncprov-sessionlog 100

dentro de la ruta de /var/lib/ldap hay crear un archivo DB_CONIFG
set_lg_bsize 2097152 --> 2G
set flags DB_LOG_AUTOREMOVE on

luego los comando seria 
slapindex -f  slapd.conf
slapindex -f DB_CONFIG

y por ultimoo reinicio del slapd

luego para terminar de tunning agregar en el crontab la linea de /usr/bin/db_archive -d -h /var/lib/ldap
que es para limpiar los log obsoletos



