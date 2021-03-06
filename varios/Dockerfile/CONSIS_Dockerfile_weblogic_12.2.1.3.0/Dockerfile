# Utilizando CentOS 7 para la base de la imagen
FROM centos:7

MAINTAINER Carlos Gomez G cgomeznt@gmail.com

# Declaramos las siguientes variables por recomendaciones d Docker
ENV     container docker

# Instalamos paquetes necesarios para la base que nos permitan administrar y hacer troubleshooting
RUN     yum -y update
RUN     yum -y install sudo \
        tar \
        gzip \
        openssh-clients \
        vi \
        find \
	net-tools \
	zip \
	unzip

# Limpiamos los temporales de yum
RUN	yum clean all

# Creamos el usuario y grupo valido para inicializar el Weblogic
RUN     groupadd oinstall
RUN     useradd -g oinstall oracle

# Creamos los directorios requeridos para copiar los archivos base, configuraciones y otras segun sea la necesidad. Tambien le otorgamos los permisos.
RUN	mkdir -p /u01/software && \
	mkdir -p /scm/EAR && \
	mkdir -p /scm/external && \
	mkdir -p /scm/scripts && \
	mkdir -p /scm/logs && \
	mkdir -p /u01/app/oracle/middleware && \
	mkdir -p /u01/app/oracle/config/domains && \
	mkdir -p /u01/app/oracle/config/applications  && \
	mkdir -p /home/qatest

# Creamos las variables para el Weblogic
ENV	export MW_HOME=/u01/app/oracle/middleware
ENV	export WLS_HOME=$MW_HOME/wlserver
ENV	export WL_HOME=$WLS_HOME

# Creamos la variable del JAVA_HOME y lo colocamos en el PATH
ENV	export JAVA_HOME=/u01/app/oracle/jdk1.8.0_77
ENV	export PATH=$JAVA_HOME/bin:$PATH

# Copiamos la version del JAVA y lo instalamos
COPY	jdk-8u101-linux-x64.rpm	/u01/software/
RUN	rpm -ivh /u01/software/jdk-8u101-linux-x64.rpm

# Copiamos los archivos base y de configuracion dentro de la imagen.
COPY	wls.rsp \
	oraInst.loc \
	basicWLSDomain7021.py \
 	fmw_12.2.1.3.0_wls.jar /u01/software/
COPY	startWebLogic.sh \
	stopWebLogic.sh \
	copylogs.sh /scm/scripts/
RUN	chown -R oracle:oinstall /u01 && \
        chown -R oracle:oinstall /scm && \
        chown -R oracle:oinstall /home/qatest && \
        chmod -R 775 /u01/ && \
        chmod -R 775 /scm/ && \
	chmod -R 775 /home/qatest


# Nos cambiamos al usuario oracle para instalar y configurar WebLogic.
USER	oracle

# Instalamos de forma desasistida el WebLogic 12.2.1.3.0
RUN 	$JAVA_HOME/bin/java -Xmx1024m -jar /u01/software/fmw_12.2.1.3.0_wls.jar -silent -responseFile /u01/software/wls.rsp -invPtrLoc /u01/software/oraInst.loc 

# Creamos y configuramos el Dominio en el puerto 7021 y los DataSource. Si queremos cambiar este comportamiento debemos editar el archivo basicWLSDomain7021.py
RUN	source /u01/app/oracle/middleware/wlserver/server/bin/setWLSEnv.sh  && \
	$JAVA_HOME/bin/java weblogic.WLST /u01/software/basicWLSDomain7021.py     

RUN	sed -i '/<configuration-version>12.2.1.3.0<\/configuration-version>/a <web-app-container>' \
	/u01/app/oracle/middleware/user_projects/domains/D7021/config/config.xml; \
	sed -i '/<web-app-container>/a <show-archived-real-path-enabled>true<\/show-archived-real-path-enabled>' \
	/u01/app/oracle/middleware/user_projects/domains/D7021/config/config.xml; \
	sed -i '/<show-archived-real-path-enabled>true<\/show-archived-real-path-enabled>/a <\/web-app-container>' \
	/u01/app/oracle/middleware/user_projects/domains/D7021/config/config.xml

RUN	sed -i -e 's/Xms512m/Xms2048m/g' /u01/app/oracle/middleware/user_projects/domains/D7021/bin/setDomainEnv.sh && \
	sed -i -e 's/Xmx512m/Xmx8192m/g' /u01/app/oracle/middleware/user_projects/domains/D7021/bin/setDomainEnv.sh && \
	sed -i -e 's/Xms256m/Xms2048m/g' /u01/app/oracle/middleware/user_projects/domains/D7021/bin/setDomainEnv.sh

# Limpiamos todos los archivo que ya no son requeridos para la imagen.
RUN	rm -rf /u01/software/*

# Creamos este volumen que nos permite modificar, eliminar o agregar archivos y/o directorios luego que el CONTENEDOR este en uso.
VOLUME	"/scm"

# Cuando el CONTENEDOR este operativo, el host expondra este puerto.
ARG	PORT=7021
EXPOSE	$PORT

#Lanzar WebLogic
#CMD ["/u01/app/oracle/middleware/user_projects/domains/D7021/bin/startWebLogic.sh"]
CMD ["/scm//scripts/startWebLogic.sh"]

