Recomendaciones de la infraestructura
=====================================

Antes de decidir la infraestructura se debe decidir la arquitectura de la aplicación.

La arquitectura definirá el tipo de servidor de aplicaciones que se va a requerir y el manejador de le base de datos.

> Para el servidor de aplicaciones, si la aplicación no usa elementos Java EE (colas de mensajes, EBJs, etc.), el servidor de aplicaciones debería ser algo ligero como Tomcat o Jetty.

Adicionalmente se deberia tener un estimado de que tan grande va a ser la base de datos y cuanto va a crecer a lo largo de un año; para poder decidir sobre el espacio necesario en disco.


Procesadores
------------

Si el sistema va a tener tanto el servidor de aplicaciones como el de base de datos, se recomienda tener al menos dos procesadores.

Si se considera que el sistema va a realizar muchas operaciones, lo ideal sería que tuviese cuatro procesadores.


Memoria
-------

La cantidad de memoria dependerá mucho de tres factores:

- Servidor de aplicaciones
- Los frameworks utilizados en la aplicación
- Si la base de datos está en el mismo servidor y lo pesada de esta

Si el servidor de aplicaciones es tomcat o jetty, la cantidad de memoria más bien dependerá de los frameworks que use la aplicación (con 2GB de RAM es suficiente para el sistema y el servidor de aplicaciones)

Si el servidor de aplicaciones de tipo EE (como Glassfish o Weblogic), lo ideal es tener un mínimo de 4GB de RAM.

Adicionalmente entrarán en juego los frameworks que use la aplicación.

Por ejemplo, si usan JPA o Hibernate, se requiere que la máquina tenga como mínumo 4GB en RAM. Por otro lado, si se usa spring core, sólo se requieren 2GB de RAM.

Por último se deberá tener en cuenta que, si la base de datos se encuentra en el mismo servidor, se deberá considerar los requisitos minimos o recomendados para la base de datos usada.


Particionado
------------

Adicional a las particiones que infraestrucutra genera, se sugiere tener separados algunos de los directorios del sistema en particiones independientes. 

> Se requiere que se use LVM para administrar las particiones del sistema, en caso de que se requiera modificar el tamaño de alguna partición en tiempo real.

La ventaja de tener particiones independientes es que, de ser necesario, el respaldo incremental es más rápido y se puede modificar el tamaño de una particion que se esté quedando sin espacio sin afectar el resto del sistema.

> En el mejor de los casos, sólo se debería separar los directorios de la base de datos y los directorios de trabajo del servidor de aplicaciones y la aplicación en particular.

Sugerencias de particionado:

| Partición     | Tamaño | Descripción |
| ------------- | ------ | ----------- |
| `/`           | 8GB    | Particion principal del sistema |
| `/var*`       | 10GB   | |
| `/usr**`      | 10GB   | |
| `/boot`       | 2GB    | Archivos para el arranque del sistema |
| _swap_        | Ver seccion _swap_ | Memoria de intercambio |
| base de datos | Ver sección DB     | Particion para la base de datos |

> En nuestro caso, no requerimos una partición para `/home` pues nuestro servidor no es un servidor de usuarios.

\* El tamaño del var dependerá mucho de si la base de datos va a tener su propia partición para el guardado de los datos o si se dejará en el lugar default.

\** El tamaño de usr dependerá mucho de que tantos paquetes del sistema o apliaciones del usaurio se instalen.

[Revisar](https://help.ubuntu.com/community/DiskSpace)

### Swap

El tamaño del swap dependerá de la cantidad de RAM, en Redhat/CentOS se recomienda de la siguiente forma:

| RAM | Mínimo | Máximo |
| --- | ------ | ------ |
| Menos de 2GB | 2 veces el tamaño de la RAM | 3 veces el tamaño de la RAM |
| De 2GB a 8GB | Lo mismo que la RAM | 2 veces el tamaño de la RAM |
| de 8GB a 64GB | Al menos 4GB | 1.5 veces el tamaño de la RAM |
| Más de 64GB | Al menos 4GB | Sin recomendación |


[Revisar](https://www.debian.org/releases/jessie/amd64/apcs03.html.en)

### Base de datos

De acuerdo al manejador de la base de datos, lo recomendable es que el directorio donde se guardan los datos de la base esté en una partición independiente y que esta partición se respalde periódicamente.

Ejemplos

| Manejador  | Directorio de datos.  |
| ---------- | --------------------- |
| Mysql      | `/var/lib/mysql`      |
| PostgreSQL | `/var/lib/pgsql/data` |
| MongoDB    | `/var/lib/mongodb`    |

> La ruta dependerá del paquete instalado y la versión del mismo.


Servidor de aplicaciones
------------------------

Si el servidor de aplicaciones se instaló por medio de un paquete, lo ideal es revisar en que directorios se guardan los datos de la aplicacion y los directorios de trabajo; para evauluar si es necesario respaldar esos directorios.

Si el servidor de aplicaciones se instala de forma manual, lo ideal es que éste se deje en algún directorio dentro de `/usr/local` y se evalue si se debe respaldar ese directorio.

### Configuración de la memoria

Por lo general la configuración de default de la memoria no nos va a funcionar ya en producción.

En conjunto con lo que recomienda IMB y Oracle, podemos sugerir lo siguiente:

| Parámetro         | Valor.           |
| ----------------- | :--------------: |
| `-Xms`            | 3/4 de la RAM    | 
| `-Xmx`            | 3/4 de la RAM    | 
| `-XX:PermSize` *  | Como mímimo 256M |
| `-XX:MaxPermSize` | El mayor entre `-XX:PermSize` y 1/8 de `-Xmx` |

> Estas recomendaciones son si el servidor no tiene otros servicios corriendo (como la base de datos), en ese caso sería tomar como RAM la RAM total menos lo que requiera al base de datos para funcionar.

> Deben tener en cuenta que el _PermSize_ es independiente a la memoria reservada por `-Xms`.

> Tomcat, sin configuraciones adicionales, no administra muy bien grandes cantidades de memoria.

\* En Java 8, el parámetro `-XX:PermSize` y `-XX:MaxPermSize` ya no están disponibles, pues cambió la forma en que se maneja la memoria.

[Revisar](https://www.ibm.com/support/knowledgecenter/es/SSEP7J_10.2.2/com.ibm.swg.ba.cognos.ig_rtm.10.2.2.doc/t_modifyingweblogicjvmrequirements.html)

[Revisar](https://docs.oracle.com/cd/E19575-01/820-7054/ghlyt/index.html)

[Revisar](http://indrayanblog.blogspot.mx/2011/03/cxv.html)


Respaldos
---------

Se ofrecen dos tipos de respaldos:

- Incremental
- Completo

Lo ideal es que se haga un respaldo completo del sistema cada semana y uno incremental por día de los directorios que tienen datos cambiantes (como el de la base de datos).

Sin embargo, el tipo de respaldo dependerá mucho del proyecto y de la criticidad del sistema.

