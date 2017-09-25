> **Autor** : Ivan Rodrigo Nievez Mondragon  
> **Correo**: ivan.nievez@infotec.mx


# Instalación y configuración de VAGRANT en CENTOS 7

### ¿Qué es VAGRANT?
Vagrant, es una herramienta en línea de comando para administrar el ciclo de vida de máquinas virtuales comunmente llamadas "cajas". Como desarrolladores, en Vagrant, podemos instalar y configurar software en una máquina vitual que nos permita simular el servidor en donde quedara alojada nuestra aplicación web.

### Instalación de VAGRANT.
- Como primer punto, debemos contar con VirtualBox instalado en nuestro equipo.
- Ahora necesitamos descargar Vagrant y seleccionar el tipo de instalador de acuerdo a nuestro sisema operativo, para hacerlo tenemos que dar [click aquí](https://www.vagrantup.com/downloads.html "Descarga de Vagrant).
- Lo que resta es ejecutar el instalador, para este caso en particular, contamos con un sistema operativo linux basado en Red Hat, Fedora Core 23, por lo tanto nuestra manera de instalarlo es la siguiente:

**Nota:** Vagrant también puede gestionar VMWare y Docker para el uso de sus "cajas"

```bash
$ sudo rpm -vhiU vagrant_1.8.6_x86_64.rpm
```
- Ahora, para corroborar que tenemos instalado correctamenta Vagrant, ejecutamos el siguiente comando:

```bash
$ vagrant -v
# Esto dará como resultado:
Vagrant 1.8.6
```
### Primer proyecto con Vagrant
Vagrant funciona con (cajas), de esta manera podemos utilizar sistemas virtualizados con determinadas herramientas.
Para nuestra primera *"caja"* la cual contendrá una máquina virtual con un sistema operativo Ubuntu Trusty de 64 bits, ejecutamos lo siquiente:
```bash
$ mkdir trusty
$ cd trusty
# Iniciamos nuestra primer caja
$ vagrant init ubuntu/trusty64
# El resultado del siguiente comando será el siguiente:
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
# Levantamos nuestra primera caja.
$ vagrant up
# La salida del comando anterior, será similar a esta:
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'ubuntu/trusty64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'ubuntu/trusty64'
    default: URL: https://atlas.hashicorp.com/ubuntu/trusty64
==> default: Adding box 'ubuntu/trusty64' (v20161014.0.0) for provider: virtualbox
    default: Downloading: https://atlas.hashicorp.com/ubuntu/boxes/trusty64/versions/20161014.0.0/providers/virtualbox.box
==> default: Successfully added box 'ubuntu/trusty64' (v20161014.0.0) for 'virtualbox'!
==> default: Importing base box 'ubuntu/trusty64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/trusty64' is up to date...
==> default: Setting the name of the VM: test_default_1476941093777_25689
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 4.3.36
    default: VirtualBox Version: 5.1
==> default: Mounting shared folders...
    default: /vagrant => /home/duser/vagrant_machines/trusty
```
Esto se conectara al contenedor de *"cajas"* de Vagrant llamado ***Atlas***, descargargando un archivo de configuración para nuestra máquina virtual mediante el primer comando y al final descargando la imagen y arrancandola, con el segundo comando.

Si deseas conocer más *"cajas"* disponibles de Vagrant, visita el siguiente link para mirar su [Atlas](https://atlas.hashicorp.com/boxes/search?utm_source=vagrantcloud.com&vagrantcloud=1 "Atlas").

Ahora conoceremos algunos de los comandos de Vagrant, para controlar nuestra maquina virtual o "caja" recién creada.
- Lavantar la máquina virtual o "caja":
```bash
$ vagrant up
```
- Para entrar por SSH a la máquina no hace falta ni saber la dirección IP. Basta con un comando:
```bash
$ vagrant ssh
```
- Poner la máquina en estado de suspensión:
```bash
$ vagrant suspend
```
- Si hemos suspendido la máquina pero queremos volver a ejecutarla, ejecutamos el siguiente comando:
```bash
$ vagrant resume
```
- Para apagar la máquina:
```bash
$ vagrant halt
```
- Para saber el estado de la máquina (apagada, ejecutándose o en modo suspensión):
```bash
$ vagrant status
```
- Para destruir la máquina:
```bash
$ vagrant destroy
```
**¡Atención! Este comando no apaga la máquina, sino que la elimina y borra todos los datos.**

# Instalación de Apache Tomcat 7

## Generales:
- Creación y configuración de nuestra "caja".
- Archivo de configuración config.sh

### Creación y configuración de nuestra "caja".

Ahora que conocemos algunos de los conceptos básicos de Vagrant, pasaremos a la configuración de nuestra propia maquina virtual, con el sistema operativo que deseemos del catálogo de Vagrant ya mencionado y el software necesario para desplegar una aplicación, para ello comenzaremos creando el archivo de configuración  ***Vagrantfile***.

Para esto, será necesario ejecutar lo siguientes comandos:
```bash
$ mkdir vagrant_getting_started
$ cd vagrant_getting_started
$ vagrant init
```
Una vez ejecutados estos comandos, nos creara un archivo llamado "Vagrantfile", en este archivo, encontramos la configuración básica para una maquina virtual o "caja" de Vagrant.

Este archivo, encontraremos una descripción y configuración de varios parámetros para configurar en nuestra "caja". Por ahora nos centaremos en ***"config.vm.box = "base""***.
Este parámetro indica la "caja" que vamos a utilizar que se encuntra en el ***Atlas*** de Vagrant.

Aquí también podemos especificar url o directorios en donde se encuentran las imagenes a utilizar, para este manual, no se utilizara ninguna de las dos opciones anteriormente mencionadas.

Las "cajas" a utilizar tienen una nomenclatura en particular para su nombrado y uso, el nombre esta divido en 2 partes, el **usuario** y el  **nombre**  de la caja, separados por una barra diagonal **/**.

En nuestro caso utilizaremos un sistema operativo Centos en su versión 7, por lo tanto el nombre de la caja será ***centos/7***.
Aquí podemos observar que el nombre del usuario es ***centos*** y su "caja" es ***7***.

Esta nomenclatura, no garantiza que el nombre de las "cajas" sea *canonico*, por ejemplo para **CentOS**, no se garantiza que en su versión 6, el nombre de la "caja" sea ***centos/6***.

Hasta aquí, nuestro archivo de configuración sin todos los comentarios, deberá lucir así:

```bash
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
end
```
Ahora iniciamos nuestra "caja" con CeontOS 7 tal y como hicimos en en ejemplo anterior.

Para acceder a nuestro "caja" funcionando, es solo basta con ejecutar el siguiente comando:

```bash
$ vagrant ssh
```

Este comando nos abrirá una sesión con privilegios de adminsitrador y poder ejecutar lo que necesitemos, pero **¡Cuidado!** puede ser tendador ejecutar lo sienguiente:

```bash
rm -rf /
```
Esto borrara todos los directorios en nuestra "caja" incluso ***/vagrat*** directorio compartido por default en Vagrant, que contiene nuestro archivo de configuraión ***Vagrantfile***.

Para cerrar nuestra conexión podemos utilizar *CTRL+D* ó nuestro clásico *logout*

Ya que mencionamos los directorios compartidos, veamos algunas configuraciones extras dentro de nuestro archivo *Vagrantfile*.

```bash
# Redirección de puertos:
# Cuando en el navegador web hagamos esto: localhost:18080,
# esto redireccionará al puerto 8080 en nuestra "caja"
config.vm.network "forwarded_port", guest: 8080, host: 18080

# Compartir archivos adicionales:
# El primer parámetro, indica la ruta local del directorio a compartir
# y la segunda, en donde será montado en nuestra "caja"
config.vm.synced_folder "/home/rodrigo/vagrant_machines/software", "/software"
```
**Nota:** para poder compartir los folders, será necesario contar con los plugins necesarios de acuerdo al proveedor de la virtualización, para virtualbox es necesario contar con los guest additions.

Para nuestro ejemplo, ubicaremos la carpeta de software al mismo nivel que nuestro archivo Vagrantfile.

Hasta aquí describimos dos configuraciones importantes de Vagrant, ahora mencionaremos una más, ya que Vagrant nos ofrece una manera de instalar sofware en nuestras "cajas" de manera automática al iniciarse, así nuestra "caja" puede ser creada, copiada y lista para usarse.

Esto es posible mediante la siguiente configuración:

```bash
# El archivo config.sh al mismo nivel que nuestro archivo Vagrantfile y contendra
# los comandos necesarios para la instación del sofware que requerimos.
config.vm.provision "shell", path: "config.sh"
```

De este modo, ya tenemos configurado nuestro archivo *Vagrantfile* con la redirección de puertos, directorios compartidos y archivo de instalación configurado.

**Nota:** El directorio /software contendra los directorios con el sofware previamente configurado, como por ejemplo los usuarios en el archivo users.xml de tomcat.

### Archivo de configuración config.sh

```bash
#!/bin/bash
echo "Archivo de configuración para JAVA y TOMCAT"
echo "Copiando directorios en su respectiva ruta de configuración"
echo "Copiando JAVA"
sudo cp /vagrant/software/java /usr -fvR
echo "Copiado"
echo "Copiando TOMCAT"
sudo cp /vagrant/software/tomcat /usr/local -fvR
echo "Copiado"
echo "Copiando archivos a profile.d/"
sudo cp /vagrant/software/profile/*sh /etc/profile.d/
echo "Copiado"
sudo source /etc/profile
echo "configuración de alternatives para java"
sudo alternatives --install /usr/bin/java java /usr/java/jdk7/bin/java 2000 \
--slave /usr/bin/jar jar /usr/java/jdk7/bin/jar \
--slave /usr/bin/javac javac /usr/java/jdk7/bin/javac \
--slave /usr/bin/javaws javaws /usr/java/jdk7/bin/javaws \
--slave /usr/bin/unpack200 unpack200 /usr/java/jdk7/bin/unpack200 \
--slave /usr/bin/orbd orbd /usr/java/jdk7/bin/orbd \
--slave /usr/bin/rmid rmid /usr/java/jdk7/bin/rmid \
--slave /usr/bin/rmiregistry rmiregistry /usr/java/jdk7/bin/rmiregistry \
--slave /usr/bin/servertool servertool /usr/java/jdk7/bin/servertool \
--slave /usr/bin/policytool policytool /usr/java/jdk7/bin/policytool \
--slave /usr/bin/pack200 pack200 /usr/java/jdk7/bin/pack200 \
--slave /usr/bin/java_vm java_vm /usr/java/jdk7/jre/bin/java_vm \
--slave /usr/bin/jcontrol jcontrol /usr/java/jdk7/bin/jcontrol \
--slave /usr/bin/tnameserv tnameserv /usr/java/jdk7/bin/tnameserv \
--slave /usr/bin/keytool keytool /usr/java/jdk7/bin/keytool

echo "Iniciando servidor TOMCAT"
sudo sh $CATALINA_HOME/bin/startup.sh
```
Ahora solo basta con iniciar un navegador web y colocar el siguiente url:

*http://localhost:18080*

Esto redireccionara al puerto 8080 de nuestra "caja" con JAVA y TOMCAT configurado.


Fuentes:
https://www.vagrantup.com/docs/getting-started/
https://geekytheory.com/tutorial-vagrant-1-que-es-y-como-usarlo/
