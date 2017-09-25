Instalación de Tomcat
=====================

### Actualización de paquetes

Antes de empezar la instalación se recomienda tener actualizado el sistema:

```bash
$ sudo yum -y update
```

posteriormente, si es que cambió la versión del kernel, podría ser necesario reiniciar el servidor:

```bash
$ sudo reboot
```

> Este paso debe ser considerado antes de realizarse; pues si es que el servidor se accede remotamente, es posible que se requiera una solicitud adicional para poder reiniciar el servidor.

### Instalación de Tomcat 8

#### Descargando Tomcat

Tomcat 8 se puede descargar de la página de [Descargas Apache Tomcat 8](https://tomcat.apache.org/download-80.cgi).

#### Descomprimiendo Tomcat

```bash
$ sudo mkdir /usr/local/tomcat
$ sudo tar zxf apache-tomcat-8.0.30.tar.gz -C /usr/local/tomcat
$ sudo ln -s /usr/local/tomcat/apache-tomcat-8.0.30 /usr/local/tomcat/current
```

> Cuando se hizo esta guía, la última versión de Tomcat 8 era la 8.0.30.

#### Eliminando aplicaciones y archivos innecesarios de Tomcat (opcional)

```bash
$ cd /usr/local/tomcat/current/webapps
$ sudo rm -rf docs examples ROOT
$ sudo rm -f /usr/local/tomcat/current/bin/*.bat
```

#### Creadno usuario de administración para Tomcat (opcional)

Para tener un usuario que pueda administrar y monitorear a Tomcat vía Web (por medio de la app `manager`), se requiere modificar el archivo `/usr/local/tomcat/current/conf/tomcat-users.xml`.

Se deden descomentar y modificar las líneas:

```xml
<!--
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="tomcat" roles="tomcat"/>
  <user username="both" password="tomcat" roles="tomcat,role1"/>
  <user username="role1" password="tomcat" roles="role1"/>
-->
```

Nuestro archivo quedó de la siguiente manera:

```xml
<?xml version='1.0' encoding='utf-8'?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
<!--
  NOTE:  By default, no user is included in the "manager-gui" role required
  to operate the "/manager/html" web application.  If you wish to use this app,
  you must define such a user - the username and password are arbitrary.
-->
<!--
  NOTE:  The sample user and role entries below are wrapped in a comment
  and thus are ignored when reading this file. Do not forget to remove
  <!.. ..> that surrounds them.
-->
  <role rolename="tomcat"/>
  <role rolename="manager-gui"/>
  <user username="tomcat-admin" password="no_se_muestra" roles="tomcat,manager-gui"/>
</tomcat-users>
```

> El password del usuario se guarda en texto plano, en su instalación tendrán que cambiar el password.

#### Cambio de *encoding* para la aplicación

Las aplicaciones requieren *UTF-8* como codificación de caracteres, por tal motivo, se requiere modificar el archivo `/usr/local/tomcat/current/conf/server.xml`.

Las líneas:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

se cambían por:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443"
           maxThreads="150"
           minSpareThreads="25"
           maxSpareThreads="75"
           enableLookups="false"
           acceptCount="100"
           disableUploadTimeout="true"
           URIEncoding="UTF-8"/>
```

> Adicionalmente se agregan configuraciones para mejorar el rendimiento del sistema.

#### Agregando biblioteca APR nativa

##### Instalando dependencias

```bash
$ sudo yum install openssl-devel
$ sudo yum groupinstall "Development Tools"
$ sudo yum install apr-devel
```

##### Compilando APR

```
$ cd /tmp
$ tar -zxvf /usr/local/tomcat/current/bin/tomcat-native.tar.gz
$ cd tomcat-native-*-src/jni/native
$ /configure --with-apr=/usr/bin/apr-1-config --with-java-home=/usr/java/current
$ make
$ sudo make install
```

#### Creando servicio para Tomcat

##### Creación de usuario de sistema para tomcat

```bash
$ sudo useradd -r -d /usr/local/tomcat/current tomcat
$ sudo chown -R tomcat:tomcat /usr/local/tomcat/apache-tomcat-*
```

##### Creando archivo init de Tomcat

Para el servicio de Tomcat, tenemos que crear el archivo `/usr/lib/systemd/system/tomcat.service` como el siguiente:

```ini
# Systemd unit file for tomcat

[Unit]
Description=Jakarta Tomcat 8 server
After=syslog.target network.target
Conflicts=glassfish.service

[Service]
Type=forking

Environment=JAVA_HOME=/usr/java/current
Environment=CATALINA_PID=/usr/local/tomcat/current/temp/tomcat.pid
Environment=CATALINA_HOME=/usr/local/tomcat/current
Environment=CATALINA_BASE=/usr/local/tomcat/current
Environment='CATALINA_OPTS=-XX:PermSize=128m -XX:MaxPermSize=512m -Xms512M -Xmx2048M -server -XX:+UseParallelGC -Djava.library.path=/usr/local/apr/lib'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/urandom'

ExecStart=/usr/local/tomcat/current/bin/startup.sh
ExecStop=/usr/local/tomcat/current/bin/shutdown.sh

User=tomcat
Group=tomcat

[Install]
WantedBy=multi-user.target
```

> En su instalación, deberán asegurarse que los valores de la variable `CATALINA_OPTS` son adecuados para la cantidad de memoria física que tiene su servidor.

##### Se instala y activa del servicio creado

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl enable tomcat
```

#### Iniciado de tomcat por primera vez

```bash
$ sudo systemctl start tomcat
$ sudo systemctl -l status tomcat
```

#### Firewall para Tomcat

```bash
$ sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
$ sudo firewall-cmd --reload
```

### Instalando apache <- incompleto

```bash
$ sudo yum -y install httpd
$ sudo systemctl enable httpd
```

### Instalación de las aplicaciones en Tomcat

Los proyectos están compuesto por varias aplicaciones (cada una de ellas es un directorio), en el manual de instalación describen la función de cada uno de ellas.

#### Método para desarrollo

En este método simplemente se copian las aplicaciones al directorio `webapps` de tomcat.

```bash
$ sudo systemctl stop tomcat
$ sudo su tomcat -c "cp -r /usr/local/dspace/dspace54/webapps/* /usr/local/tomcat/current/webapps"
$ sudo systemctl start tomcat
```

> La primera vez que se iniica el servidor puede tardar más de lo habitual debido a que se genera la base de datos.

#### Método para producción <- incompleto

Este método crea un archivo de configuración por aplicación y permite modificar las opciones de cache del servidor de aplicaciones.

```bash
$ sudo systemctl stop tomcat
...
$ sudo systemctl start tomcat
```

### Comprobación de la instalación

Una vez terminado la inicialización de tomcat (esto se puede ver en el archivo de *log* de tomcat: `/usr/local/tomcat/current/logs/catalina.out`), podemos acceder a la página inicial de DSpace `http://<IP_DEL_SERVIDOR>:8080/xmlpui`.

> Antes de crear el usuario administrador, asegúrese que el servicio de tomcat inició correctamente.

### Creando usuario administrador

Para crear el usuario administrador, tendremos que ejecutar un comando que se encuentra en el directorio de instalación de DSPace con el usuario del servidor de aplicaciones:

```bash
$ sudo su tomcat -c "/usr/local/dspace/dspace54/bin/dspace create-administrator"

Creating an initial administrator account
E-mail address: admin@dspacelocal.com
First name: Usuario
Last name: Administrador
Password will not display on screen.
Password:
Again to confirm:
Is the above data correct? (y or n): y
Administrator account created
```
