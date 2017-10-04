# CoreOS

> **Autor** : Ivan Rodrigo Nievez Mondragon  
> **Correo**: ivan.nievez@infotec.mx

## ¿Qué es CoreOS?
CoreOS es un sistema operativo ligero de código abierto basado en el kernel de Linux y diseñado para proporcionar la infraestructura para los despliegues en clúster. También podemos decir que *CoreOS* es un linux para despliegues masivos, para poder hacer esto, utiliza 3 tecnologías claves:
- Docker.
- Fleet.
- etcd

Ya que conocemos que es CoreOS veamos como crear una máquina virtual con vagrant.

### Pre-requisitos.
- Haber leiodo el manual de introducción a vagrant.
- Contar con Vagrant instalado.
- Contar con VirtualBox instalado.
- Contar con Git instalado.
- Contar con acceso a internet para descargar la imágen de CoreOs.

### Configurando el entorno de pruebas.
Vamos a clonar la imágen de *CoreOS* desde su sitio oficial de github.
```bash
$ git clone https://github.com/coreos/coreos-vagrant.git
$ cd coreos-vagrant
```
Lo que hacen los comando de arriba, es conectarse a github y clonar los archivos de configuración para poder levantar una maquina virtual con CoreOS, los archivos descargados son:
```bash
$ ls -l
total 52
-rw-rw-r--. 1 rodrigo rodrigo  3378 Oct 22 23:29 config.rb.sample
-rw-rw-r--. 1 rodrigo rodrigo  2448 Oct 22 23:29 CONTRIBUTING.md
-rw-rw-r--. 1 rodrigo rodrigo  1422 Oct 22 23:29 DCO
-rw-rw-r--. 1 rodrigo rodrigo 11325 Oct 22 23:29 LICENSE
-rw-rw-r--. 1 rodrigo rodrigo   104 Oct 22 23:29 MAINTAINERS
-rw-rw-r--. 1 rodrigo rodrigo   126 Oct 22 23:29 NOTICE
-rw-rw-r--. 1 rodrigo rodrigo  4150 Oct 22 23:29 README.md
-rw-rw-r--. 1 rodrigo rodrigo  1349 Oct 22 23:29 user-data.sample
-rw-rw-r--. 1 rodrigo rodrigo  4900 Oct 22 23:29 Vagrantfile
```
El primer archivo a configurar es el *config.rb.sample*, en primer lugar, realizaremos una copia con el nombre *config.rb* y ahora cambiaremos los sigueintes parámetros con nuestro editor de texto preferido:
Número de instancias que queremos levantar en nuestro cluster vía Vagrant. en nuestro caso solo requerimos de una.
```
$num_instances=1
```
El canal oficial de CoreOS desde donde bajaremos la versión a utilizar en nuestra plataforma
```
$update_channel='stable'
```
Otras configuraciones *opcionales* que nos ayudarán a defir la capacidad de nuestras máquinas son:
```
$vb_gui = false  
$vb_memory = 1024
$vb_cpus = 1
```
Soló restar guardar cambios y terminamos con nuesra edición.

Configuraciones para ediciones previas de config.rb, al momento de realizar este tutorial, la configuración es automática, al encontrar la sigueinte línea en config.rb.
```
# Automatically replace the discovery token on 'vagrant up'
```

****
Pasaremos al siguiente archivo llamado "user-data.sample", de igual manera, realizamos una copia del archivo con el siguiente nombre "user-data", aquí cambiaremos el siguiente parámetro.
```
discovery: https://discovery.etcd.io/
```
CoreOS permite configurar valores del clúster al momento de iniciar las instancias, para esto utiliza el sistema de cloud-config, en nuestro caso, podemos proveer esta información a nuestras instancias vía el archivo de configuración user-data. Uno de los parámetros mas importantes que utilizaremos será el discovery url, esto es una URL que nuestra instancia de etcd utilizara para almacenar información sobre nuestro clúster y elegir un líder inicial del clúster de etcd, así como también mantener configuraciones de nuestros aplicativos en pasos posteriores. Para esto deberemos obtener una dirección de discovery desde la siguiente [URL](https://discovery.etcd.io/new.).
Para este tutorial nos genero:
```
https://discovery.etcd.io/473220db732863a41e2a7a47af1b342e
```
y sustituimos:
```
discovery: https://discovery.etcd.io/473220db732863a41e2a7a47af1b342e
```

****
Ahora levantamos nuestra máquina vitual con CoreOS
```bash
$ vagrant up
```
Esto descargará y configurará CoreOs dando la siguiente salida en consola:
```bash
Bringing machine 'core-01' up with 'virtualbox' provider...
==> core-01: Box 'coreos-stable' could not be found. Attempting to find and install...
    core-01: Box Provider: virtualbox
    core-01: Box Version: >= 0
==> core-01: Loading metadata for box 'https://storage.googleapis.com/stable.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json'
    core-01: URL: https://storage.googleapis.com/stable.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json
==> core-01: Adding box 'coreos-stable' (v1122.3.0) for provider: virtualbox
    core-01: Downloading: https://stable.release.core-os.net/amd64-usr/1122.3.0/coreos_production_vagrant.box
    core-01: Calculating and comparing box checksum...
==> core-01: Successfully added box 'coreos-stable' (v1122.3.0) for 'virtualbox'!
==> core-01: Importing base box 'coreos-stable'...
==> core-01: Matching MAC address for NAT networking...
==> core-01: Checking if box 'coreos-stable' is up to date...
==> core-01: Setting the name of the VM: coreos-vagrant_core-01_1477201787390_63157
==> core-01: Clearing any previously set network interfaces...
==> core-01: Preparing network interfaces based on configuration...
    core-01: Adapter 1: nat
    core-01: Adapter 2: hostonly
==> core-01: Forwarding ports...
    core-01: 22 (guest) => 2222 (host) (adapter 1)
==> core-01: Running 'pre-boot' VM customizations...
==> core-01: Booting VM...
==> core-01: Waiting for machine to boot. This may take a few minutes...
    core-01: SSH address: 127.0.0.1:2222
    core-01: SSH username: core
    core-01: SSH auth method: private key
==> core-01: Machine booted and ready!
==> core-01: Setting hostname...
==> core-01: Configuring and enabling network interfaces...
==> core-01: Running provisioner: file...
==> core-01: Running provisioner: shell...
    core-01: Running: inline script
```
Nos conectamos a la máquina.

```bash
$ vagrant ssh core-01 -- -A
```

Con esto hemos terminado este tutorial de intriducción a CoreOS.


Fuentes:
https://coreos.com/docs/
https://es.wikipedia.org/wiki/CoreOS
http://blog.hostdime.com.co/linux-para-una-red-cloud-y-centro-de-datos-con-coreos/
