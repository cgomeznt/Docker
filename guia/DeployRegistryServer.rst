Deploy a Registry Server
=======================

https://docs.docker.com/registry/deploying/

**Run a local registry** Usar el siguiente comando para instanciar un contenedor de registry::

	$ docker run -d -p 5000:5000 --restart=always --name registry registry:2

Pull the ubuntu:16.04 image from Docker Hub.::

	$ docker pull ubuntu:16.04

Tag the image as localhost:5000/my-ubuntu. This creates an additional tag for the existing image. When the first part of the tag is a hostname and port, Docker interprets this as the location of a registry, when pushing.::

	$ docker tag ubuntu:16.04 localhost:5000/my-ubuntu

Push the image to the local registry running at localhost:5000::

	$ docker push localhost:5000/my-ubuntu

Remove the locally-cached ubuntu:16.04 and localhost:5000/my-ubuntu images, so that you can test pulling the image from your registry. This does not remove the localhost:5000/my-ubuntu image from your registry.::

	$ docker image remove ubuntu:16.04
	$ docker image remove localhost:5000/my-ubuntu

Run an externally-accessible registry
+++++++++++++++++++++++++++++++++++++

Running a registry only accessible on localhost has limited usefulness. In order to make your registry accessible to external hosts, you must first secure it using TLS.

This example is extended in Run the registry as a service below.

Debemos crear los certificados primero con **openssl**

https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309

Create Root Key::

	openssl genrsa -des3 -out rootCA.key 4096

Create and self sign the Root Certificate::

	openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt

Create a certificate (Done for each server) - Create the certificate key::

	openssl genrsa -out registry.key 2048

Create the signing (csr) ::

	openssl req -new -key registry.key -out registry.csr

Verify the csr's content::

	openssl req -in registry.csr -noout -text

Generate the certificate using the registry csr and key along with the CA Root key::

	openssl x509 -req -in registry.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out registry.crt -days 500 -sha256

Verify the certificate's content::

	openssl x509 -in registry.crt -text -noout

Luego que tengamos los certificados los debemos crear una carpeta en los servidores que vayan a utilizar el Docker Registry::

	mkdir -p /etc/docker/certs.d/NOMBRE_DEL_CONTENEDOR\:PUERTO/

	mkdir -p /etc/docker/certs.d/registry\:5000/

Y dentro de esa carpeta debemos copiar el certificado del servidor y la CA que lo firmo::

	cp registry.crt rootCA.crt /etc/docker/certs.d/NOMBRE_DEL_CONTENEDOR\:PUERTO/

	cp registry.crt rootCA.crt /etc/docker/certs.d/registry\:5000/

Se instancia el contenedor de Registry::

	docker run -d \
	  --restart=always \
	  --name registry \
	  -v "$(pwd)"/certs:/certs \
	  -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 \
	  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/registry.crt \
	  -e REGISTRY_HTTP_TLS_KEY=/certs/registry.key \
	  -p 5000:500 \
	  --network app \
	  registry:2

Vemos que imágenes tiene el Registry::

	curl -k https://registry:5000/v2/_catalog


descargamos una imagen de prueba::

	docker pull alpine

	docker tag alpine:latest registry:5000/alpine

	docker push registry:5000/alpine

Volvemos a verificar que imágenes tiene el Registry::

	curl -k https://registry:5000/v2/_catalog
