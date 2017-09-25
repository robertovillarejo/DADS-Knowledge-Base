# Docker

> **Autor** : Ivan Rodrigo Nievez Mondragon  
> **Correo**: ivan.nievez@infotec.mx


## ¿Que es Docker?

Docker es una tecnología de código abierto que le permite crear, ejecutar, probar e implementar aplicaciones distribuidas dentro de contenedores de software. Le permite empaquetar una aplicación en una unidad estandarizada para el desarrollo de software, que contiene todo lo que necesita para funcionar: código, tiempo de ejecución, herramientas y bibliotecas del sistema, etc. Docker le permite implementar las aplicaciones de forma rápida, fiable y sistemática, en cualquier entorno.
Además proporciona una capa adicional de abstracción y automatización de Virtualización a nivel de sistema operativo en Linux. Docker utiliza características de aislamiento de recursos del kernel de Linux, tales como cgroups y espacios de nombres (namespaces) para permitir que "contenedores" independientes se ejecuten dentro de una sola instancia de Linux, evitando la sobrecarga de iniciar y mantener máquinas virtuales.

### Instalación de Docker.
Para nuestra instalación, se tienen las siquientes consideraciones:

- Sistema operativo Fedora Core 23.
- Versión del Kernel 4.7.7-100.fc23.x86_64
- Instalación de Docker mediante dnf.

Docker, ofrece dos tipos de instalación, una de ellas es mediante la configuración de un repositorio, la cual utilizaremos para este manual y una, mediante un script de shell.

Primero debes asegurarnos de que contamos con los paquetes del sistema actualizados, por tanto, debemos acceder a la consola con un usuario con privilegios de administrador o como root y ejecutar los siguientes comandos.

```bash
$ sudo dnf -y update
```

Una vez hecho lo anterior, configuraremos el repositorio de Docker.

```bash
$ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/fedora/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```
Ahora instalamos Docker.

```bash
$ sudo dnf -y install docker-engine
```
Al terminar la instalación, habilitaremos el servicio de Docker, para que se inicie la próxima vez que iniciemos el sistema y levantamos el servicio, para poder utilizarlo.

```bash
$ sudo systemctl enable docker.service
$ sudo systemctl start docker
```
Ahora verificamos que todo este funcionando correctamente.

```bash
 $ sudo docker run --rm hello-world

 Unable to find image 'hello-world:latest' locally
 latest: Pulling from library/hello-world
 c04b14da8d14: Pull complete
 Digest: sha256:0256e8a36e2070f7bf2d0b0763dbabdd67798512411de4cdcf9431a1feb60fd9
 Status: Downloaded newer image for hello-world:latest

 Hello from Docker!
 This message shows that your installation appears to be working correctly.

 To generate this message, Docker took the following steps:
  1. The Docker client contacted the Docker daemon.
  2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
  3. The Docker daemon created a new container from that image which runs the
     executable that produces the output you are currently reading.
  4. The Docker daemon streamed that output to the Docker client, which sent it
     to your terminal.

 To try something more ambitious, you can run an Ubuntu container with:
  $ docker run -it ubuntu bash

 Share images, automate workflows, and more with a free Docker Hub account:
  https://hub.docker.com

 For more examples and ideas, visit:
  https://docs.docker.com/engine/userguide/
```

Por último, agregaremos a nuestro usurio al grupo ***docker***, este grupo se crea en automático durante la instalación de Docker. Haciendo esto, podemos presindir del commando ***sudo*** para poder ejecutar *doker*.

La razón de haer esto, es por que el demonio (o servicio) de Docker, se liga a una instancia de socket TCP de Unix y por default este socket es propiedad del super usuario (root) y por eso tenemos que utilizar *sudo* para poder ejecutar el comando *docker*, esto se logra mediante:

```bash
$ sudo usermod -aG docker your_username`
```

Al agregar nuestro usuario al grupo, será necesario cerrar sesión y volver a loguearnos para que se apliquen los cambios y podamos ejecutar *docker* sin necesidad de ***sudo***. Para verificarlo ejecuetamos el siguiente comando:

```bash
 $ docker run --rm hello-world
```

### Aprendamos sobre las imagenes y contenedores.

En Dockers, se manejan dos terminos ***"imágenes"*** y ***"contenedores"***, una ***imágen*** es un sistema base o bien, una plantilla sobre la cual trabajaremos, por otro lado, un ***contenedor*** es la instancia de una ***imagen*** con la cual podremos ejecutar y trabajar en ella.
Gracias a Docker se puede trabajar con varias instancias de la misma *imágen* y cada una de ellas contar con sus propias configuraciones. Esto se logra atravez del siguiente comando:

```bash
$ docker run hello-world
# En donde:
# docker: comando de ejecución.
# run: subcomando que crea y ejecuta un contenedor de Docker
# hello-world: indica a Docker, la imágen a instanciar dentro del contenedor.
```
Ahora que contamos con la instalación de Docker y conocemos algunos de sus conceptos básicos, relicemos un ejemplo.

### Ejecución de la imágen *"whalesay"*

Como se ha mencionado anteriormente, las *imágenes* son sistemas base con los cuales podremos trabajar, Docker cuenta con un centro de imágenes llamado [Docker Hub](https://hub.docker.com/ "Docker Hub"), en donde podemos encontrar platillas creadas por usuarios y oficiales por parte de algunos proveedores de software, por ejemplo para Apache Tomcat, Glassfish, Fedora, Ubuntu, CentOS, entre otros.
Aquí no es necesario registrarse para poder navegar en busqueda de las imágenes, durante la realización de este tutorial, en la parte inferior de la página encontramos el siguiente link con el nombre [Browse](https://hub.docker.com/explore/ "Browse"), para navegar entre una gran variedad de *imágenes*.
Para este tutorial, en la barra de busqueda, introducimos el nombre "whalesay" y al presionar enter, nos traera los resultados que coinciden, en nuesro caso nos enfocaremos en la que diga "docker/whalesay", al dar click en nombre, nos dara una descripción detallada de la imagen.

Ahora, para hacer uso de esta imágen ejecutamos los siguientes comandos:

```bash
$ docker run docker/whalesay cowsay boo
# Esto dará como resultado lo siguiente:
Unable to find image 'docker/whalesay:latest' locally
 latest: Pulling from docker/whalesay
 e9e06b06e14c: Pull complete
 a82efea989f9: Pull complete
 37bea4ee0c81: Pull complete
 07f8e8c5e660: Pull complete
 676c4a1897e6: Pull complete
 5b74edbcaa5b: Pull complete
 1722f41ddcb5: Pull complete
 99da72cfe067: Pull complete
 5d5bd9951e26: Pull complete
 fb434121fc77: Already exists
 Digest: sha256:d6ee73f978a366cf97974115abe9c4099ed59c6f75c23d03c64446bb9cd49163
 Status: Downloaded newer image for docker/whalesay:latest
  _____
 < boo >
  -----
     \
      \
       \
                     ##        .
               ## ## ##       ==
            ## ## ## ##      ===
        /""""""""""""""""___/ ===
   ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
        \______ o          __/
         \    \        __/
           \____\______/
```
En primer lugar, Docker busca de manera local la imágen "whaleasy" y al no encontrarla, la busca en [Docker Hub](https://hub.docker.com/ "Docker Hub"), descargandola y haciendo una instancia de ella.
Si soló queremos descargar alguna imágen basta con hacer lo siguiente:
```bash
$ docker pull docker/whalesay
```
Para listar las imagenes con las que contamos, ejecutamos el siguiente comando:
```bash
$ docker images
# Esto dará el siguinte resultado
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              c54a2cc56cbb        3 months ago        1.848 kB
docker/whalesay     latest              6b362a9f73eb        17 months ago       247 MB
```
Hasta ahora hemos trabajado con imagenes ya confguradas y listas para usar, pero ¿Qué pasa si queremos customisar nuestra propia imágen?. Pues bien, ahora utilizaremos un sistema base Ubuntu y configuraremos un servidor Apache.
En nuestra consola ejecutamos los siguientes comandos:
```Bash
$ mkdir docker_ubunti
$ cd docker_ubuntu
$ touch DockerFile
```
Contenido de Dockerfie:
```Bash
# Indicamos la imagen que utilizaremos como base, en este caso. Ubuntu
FROM ubuntu
# Indicamos el nombre del autor de la imagen
MAINTAINER User
# RUN nos ayuda a ejecutar comandos en la imagen base.
RUN apt-get update
RUN apt-get install apache2 -y
RUN echo "<h1>Apache with Docker</h1>" > /var/www/html/index.html
# Esponemos los puertos que deseamos para ser mapeados al ejecutar el contenedor
EXPOSE 80
# Indica que se ejecute un comando, cada vez que arranquemos el contenedor.
ENTRYPOINT apache2ctl -D FOREGROUND
```

Ahora construiremos nuestra imágen
```bash
$ docker build -t username/apache .
```
***Nota:*** *username/apache* es el identificador para nuestra imágen. Al final de cada line se requiere un **"."**

Para listar las imágenes disponible ejecutamos el siguiente comando:
```bash
$ docker images
```
Creamos un contenedor de nuestra imágen:
```bash
$ docker run --name apache -d -p 90:80 username/apache
```

Es tiempo de realizar una prueba para saber si esta funcionando nuestro servidor, una de ellas es hacerla mediante un navegador web escribiendo lo siguiente:
```html
http://localhost:90
```
O desde la línea de comandos:
```bash
$ curl http://localhost:90
# Esta será su salida:
<h1>Apache with Docker</h1>
```
Así es como tenemos ya configurado nuestro servidor Apache y con los puertos redireccionados. pero que pasaría si realizamos cambios y queremos mantenerlos disponibles el algún futuro para realizar algunas otras tareas. Esto se soluciona guardando los cambios en una nueva imágen que podemos utilizar para crear nuevos contenedores.
Esto lo podemos hacer mediante:
```html
docker commit -m "<comentario>" -a "<autor>" <id_contenedor> <nombre_imagen>:<etiqueta_imagen>
```

```bash
$ docker commit -m "modificaciones al contenedor apache" -a "user" docker-apache user/apache_modificado
```
De esta manera terminamos con el tutorial de introduccion a Docker.

### Otros comandos.

- Detener un contenedor
```bash
$ docker stop <contenedor>
```
- Eliminar un contenedor
```bash
$ docker rm <contenedor>
```

- Eliminar una imagen
```bash
$ docker rmi <imagen>
```
- Listar los contenedores activos
```bash
$ docker ps
```
- Listar los contenedores activos con detalle
```bash
$ docker ps -a
```

Fuentes:
https://docs.docker.com/engine/getstarted/
https://docs.docker.com/engine/reference/commandline/
https://www.adictosaltrabajo.com/tutoriales/docker-for-dummies/
https://docs.joyent.com/public-cloud/instances/connecting/connecting-to-docker
