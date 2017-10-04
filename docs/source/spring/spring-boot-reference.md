# Spring Boot: Guía de referencia

[Documento original](https://docs.spring.io/spring-boot/docs/current/reference/pdf/spring-boot-reference.pdf)

## Características de Spring Boot

### **29. Trabajando con bases de datos SQL**

El Framework Spring provee un extenso soporte para trabajar con bases de datos SQL. Desde acceso JDBC directo usando _JdbcTemplate \_hasta tecnologías completas de 'mapeo objeto-relacional' tales como Hibernate. Spring Data provee un nivel adicional de funcionalidad, creando implementaciones de \_Repository_ directamente de las interfaces y usando convenciones para generar consultas \(_queries_\) a partir del nombre de tus métodos.

### **29.1 Configurar una Fuente de Datos**

La interfaz de Java _javax.sql.Datasource \_provee un método estándar para trabajar con conexiones de bases de datos. Tradicionalmente un \_DataSource _\(Fuente de Datos\) utiliza una URL junto con algunas credenciales para establecer una conexión a la base de datos.

> Tip
>
> Revisa también la sección 'How to' para ejemplos más avanzados, normalmente para tomar el control completo sobre la configuración del _Data Source _\(Fuente de Datos\).

### Soporte de Bases de Datos _Embebidas_

Con frecuencia es conveniente desarrollar aplicaciones utilizando una base de datos \_embebida \_en-memoria. Obviamente, las bases de datos en-memoria no proveen almacenamiento persistente; necesitarás poblar tu base de datos cuando tu aplicación inicie y estar preparado para desechar tus datos cuando tu aplicación finalice.

> Tip
>
> La sección 'How to' incluye una sección sobre cómo inicializar una base de datos

Spring Boot puede auto-configurar las bases de datos H2, HSQL y Derby. No tienes que proveer ninguna URL de conexión, simplemente incluye una dependencia de construcción \(_build dependency_\) a la base de datos embebida que quieres utilizar.

> Tip
>
> Si estás usando esta característica en tus pruebas, podrás notar que la misma base de datos es reusada por tu suite de pruebas completa independientemente del número de contextos de aplicación que uses. Si quieres asegurarte de que cada contexto tenga una base de datos separada, deberías establecer \_spring.datasource.generate-unique-name \_a **true**.

Por ejemplo, las dependencias típicas de un POM serían:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.hsqldb</groupId>
    <artifactId>hsqldb</artifactId>
    <scope>runtime</scope>
</dependency>
```

> Nota
>
> Necesitas una dependencia en _spring-jdbc \_para que una base de datos embebida sea auto-configurada. En este ejemplo ésta es extraída transitivamente via \_spring-boot-starter-data-jpa_.
>
> Tip
>
> Si, por alguna razón, configuras la URL de conexión para una base de datos embebida, se debe tener cuidado de asegurarse que el apagado automático está deshabilitado. Si estás usando H2 debes usar DB\_CLOSE\_ON\_EXIT=FALSE para hacerlo. Si estás usando HSQLDB, debes asegurarte que _shutdown=true_ no se está usando. Disabling the database’s automatic shutdown allows  
>  Spring Boot to control when the database is closed, thereby ensuring that it happens once access  
>  to the database is no longer needed.

#### Conexión a una base de datos de producción

Las conexiones a bases de datos de producción también pueden ser auto-configuradas utilizando un conjunto de Fuentes de Datos \(_pooling DataSource\)_. Aquí se muestra el algoritmo para elegir una implementación específica:

* Preferimos el \_pooling DataSource \_de Tomcat por su rendimiento y concurrencia, así que si está disponible siempre lo elegimos.
* De otro modo, si HikariCP está disponible lo usaremos.
* Si ni el _pooling DataSource_ de Tomcat ni HikariCP están disponibles pero Commons DBCP sí entonces lo usaremos, pero no lo recomendamos en producción y su soporte está obsoleto.
* Finalmente, si Commons DBCP2 está disponible lo usaremos.

Si usas los _starters_ **spring-boot-starter-jdbc** o **spring-boot-starter-data-jpa** automáticamente obtendrás una dependencia de **tomcat-jdbc**.

> Nota
>
> Puedes omitir este algoritmo completamente y especificar el _connection pool_ a utilizar modificando la propiedad _spring.datasource.type_. Esto es especialmente importante si estás corriendo tu aplicación en un contenedor Tomcat como el _tomcat-jdbc_ que se provee por defecto.
>
> Tip
>
> Adicionalmente los pool de conexiones siempre pueden ser configuradas manualmente. Si defines tu propio \_bean DataSource \_no se hará la autoconfiguración.

La configuración del _DataSource_ está controlada por las propiedades de configuración externas en _spring.datasource.\*._ Por ejemplo, podrías declarar la siguiente sección en _application.properties_:

```properties
spring.datasource.url=jdbc:mysql://localhost/test
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

> Nota
>
> Deberás al menos especificar la url usando la propiedad \_spring.datasource.url \_o Spring Boot tratará de auto-configurar una base de datos embebida.
>
> Tip
>
> A menudo no tendrás que especificar el _driver-class-name_ puesto que Spring Boot puede deducirlo para la mayoría de las bases de datos a partir de la url.
>
> Para que un \_pooling DataSource \_sea creado necesitamos ser capaces de verificar que una clase de \_Driver \_está disponible, así que revisamos eso antes que nada. Por ejemplo, si estableces \_spring.datasource.driver-class-name=com.mysql.jdbc.Driver \_entonces esa clase debe poderse cargar.

Véase DataSourceProperties para conocer más de las opciones soportadas. Estas son las opciones estándar que funcionan independientemente de la implementación real. También es posible afinar las configuraciones de la implementación específica usado sus respectivos prefijos \(_spring.datasource.tomcat.\*_, _spring.datasource.hikari.\*_, y _spring.datasource.dbcp2.\*_\) Refiérase a la documentación de la implementación del \_pool \_de conexiones que estás usando para más detalles.

Por ejemplo, si estás usando el _Tomcat connection pool_ podrías personalizar varias configuraciones adicionales:

```properties
# Number of ms to wait before throwing an exception if no connection is available.
spring.datasource.tomcat.max-wait=10000
# Maximum number of active connections that can be allocated from this pool at the same time.
spring.datasource.tomcat.max-active=50
# Validate the connection before borrowing it from the pool.
spring.datasource.tomcat.test-on-borrow=true
```

#### Conexión a una Fuente de Datos \(DataSource\) JNDI

Si estás desplegando tu aplicación Spring Boot en un Servidor de Aplicaciones podrías querer configurar y administrar tu _DataSource_ usando las características incorporadas de tus Servidores de Aplicaciones y acceder usando JNDI.

La propiedad _spring.datasource.jndi-name \_puede ser usada como una alternativa a  las propiedades \_spring.datasource.url_, _spring.datasource.username_ y _spring.datasource.password_ para acceder al _DataSource_ desde una locaciónn específica JNDI. Por ejemplo, la siguiente sección en _application.properties_ muestra cómo puedes acceder al _DataSource _ de un Servidor de Aplicaciones _JBoss:_

```properties
spring.datasource.jndi-name=java:jboss/datasources/customers
```

### 29.2 Usando JdbcTemplate

Las clases _JdbcTemplate \_y \_NamedParameterJdbcTemplate_ son auto-configuradas y puedes _cablearlos_ directamente en tus propios _beans_ utilizando _@Autowire_:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

@Component
public class MyBean {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public MyBean(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    // ...
}
```

### 29.3 JPA y 'Spring Data'

La API de Persistencia de Java \(JPA\) es una tecnología estándar que te permite 'mapear' objectos a bases de datos relacionales. El POM de _spring-boot-starter-data-jpa_ provee una manera rápida de iniciar. Éste provee las siguientes dependencias clave:

* Hibernate --- Una de las implementaciones de JPA más populares.
* Spring Data JPA --- Hace más fácil implementar los repositorios basados en JPA.
* Spring ORMs --- El soporte del núcleo ORM del \_Framework \_Spring

> Tip
>
> No entraremos en muchos detaller de JPA o Spring Data aquí. Puedes seguir la guía 'Accediendo a Datos con JPA' en spring.io y leer la documentación de referencia Spring Data JPA y Hibernate.
>
> Note
>
> Por defecto, Spring Boot utiliza Hibernate 5.0.x. Sin embargo, también es posible usar 4.3.x o 5.2.x si lo deseas. Por favor consulta los ejemplos Hibernate4 y Hibernate5 para ver cómo hacerlo.

#### Clases de Entidades

Tradicionalmente, las clases de 'Entidad' JPA se especifican en un archivo _persistence.xml_. Con Spring Boot este archivo no es necesario y en su lugar se utilizar el 'Escaneo de Entidades' \(_Entity Scanning\). \_Por defecto se buscan todos los paquetes que se encuentran debajo de tu clase de configuración principal \(la que tiene la anotación _@EnableAutoConfiguration_ o _@SpringBootApplication\_\).

Se considerará cualquier clase anotada con _@Entity_, _@Embeddable \_o _@MappedSuperclass. \_Una clase de entidad típica se vería algo así:

```java
package com.example.myapp.domain;
import java.io.Serializable;
import javax.persistence.*;

@Entity
public class City implements Serializable {
    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private String state;

    // ... additional members, often include @OneToMany mappings

    protected City() {
    // no-args constructor required by JPA spec
    // this one is protected since it shouldn't be used directly
    }

    public City(String name, String state) {
        this.name = name;
        this.country = country;
    }

    public String getName() {
       return this.name;
    }

    public String getState() {
        return this.state;
    }

    // ... etc
}
```

> Tip
>
> Puedes personalizar las ubicaciones donde se realiza el escaneo de entidades utilizando la anotación _@EntityScan_. Mira la sección 77.4, cómo "Separar las definiciones _@Entity_ de la configuración de Spring".

#### Repositorios JPA de Spring Data

Los repositorios JPA de Spring Data son interfaces que te permiten definir el acceso a los datos. Las consultas JPA son creadas automáticamente a partir del nombre de tus métodos. Por ejemplo, una interfaz _CityRepository_ podría declarar un método \_findAllByState\(String state\) \_encontrar todas las ciudades en un estado proporcionado.

Para consultas más complejas puedes anotar tu método utilizando la anotación _Query_ de Spring Data.

Los repositorios de Spring Data usualmente extienden de las interfaces _Repository_ o _CrudRepository_. Si estás utilizando la auto-configuración, los repositorios se buscarán a partir  del paquete que contiene tu clase principal de configuración \(la que está anotada con _@EnableAutoConfiguration_ o con _@SpringBootApplication_\).

Éste es un repositorio típico de Spring Data:

```java
package com.example.myapp.domain;

import org.springframework.data.domain.*;
import org.springframework.data.repository.*;

public interface CityRepository extends Repository<City, Long> {
    Page<City> findAll(Pageable pageable);
    City findByNameAndCountryAllIgnoringCase(String name, String country);
}
```

> Tip
>
> Apenas y se ha tocado el tema de Spring Data JPA. Para detalles completos revisa su documentación de referencia.

#### Creando y tirando bases de datos JPA

Por defecto, las bases de datos JPA se crearán automáticamente **solo** si utilizar una base de datos embebida \(H2, HSQL o Derby\). Puedes configurar explícitamente los ajustes de JPA usando las propiedades _spring.jpa.\*_. Por ejemplo, para crear y tirar tablas puedes añadir lo siguiente en tu archivo _application.properties_.

```
spring.jpa.hibernate.ddl-auto=create-drop
```

> Nota
>
> La propiedad interna propia de Hibernate para hacer esto es _hibernate.hbm2ddl.auto_. Puedes establecerla, junto con otras propiedades nativas de Hibernate, utilizando _spring.jpa.properties.\* _\(el prefijo se elimina antes de añadirlas al _entity manager_\). Ejemplo:

```
spring.jpa.properties.hibernate.globally_quoted_identifiers=true
```

pasa a \_hibernate.globally\_quoted\_identifiers \_al entity manager de Hibernate.

Por defecto la ejecución  \(o validación\) DDL se aplaza hasta que se inicia el _ApplicationContext_. También hay una bandera _spring.jpa.generate-ddl_, pero no es usada si la auto-configuración de Hibernate se encuentra activa porque las configuraciones _ddl-auto_ son más granulares.

#### Open EntityManager in View

Si estás ejecutando una aplicación web, Spring Boot registrará por defecto al OpenEntityManagerInViewInterceptor_ \_para aplicar el patrón "Open EntityManager in View", por ejemplo para permitir la carga lenta \(\_lazy loading_\) en las vistas web. Si no deseas tener este comportamiento entonces debes establecer _spring.jpa.open-in-view_ en _false_ en tu _application.properties_.

## 30. Trabajando con tecnologías NoSQL

Spring Data provee proyectos adicionales que te ayudan a acceder a una variedad de tecnologías NoSQL incluyendo MongoDB, Neo4J, Elasticsearch, Solr, Redis, Gemfire, Cassandra, Couchbase y LDAP. Spring Boot provee auto-configuración para Redis, MongoDB, Neo4j, Elasticsearch, Solr Cassandra, Couchbase y LDAP; puedes hacer uso de otros proyectos, pero necesitarás configurarlos tú mismo. Consulta la documentación de referencia apropiada en projects.spring.io/spring-data.

### 30.1 Redis

### 30.2 MongoDB

MongoDB es una base de datos de documentos NoSQL de código abierto que utiliza un esquema parecido a JSON en lugar de los tradicionales datos relacionales basados en tablas. Spring Boot ofrece varias conveniencias para trabajar con MongoDB, incluyendo el 'Starter' _spring-boot-starter-data-mongo-db_.

#### Conectando a una base de datos MongoDB

Puedes inyectar una auto-configurada _org.springframework.data.mongodb.MongoFactory \_para acceder a bases de datos Mongo. Por defecto la instancia tratará de conectarse a un servidor MongoDB utilizando la URL \_mongodb://localhost/test_:

```java
import org.springframework.data.mongodb.MongoDbFactory;
import com.mongodb.DB;

@Component
public class MyBean {
    private final MongoDbFactory mongo;

    @Autowired
    public MyBean(MongoDbFactory mongo) {
        this.mongo = mongo;
    }

    // ...

    public void example() {
        DB db = mongo.getDb();
        // ...
    }
}
```

Puedes establecer la propiedad _spring.data.mongodb.uri_ para cambiar la URL y configurar ajustes adicionales tales como el conjunto de réplica \(_replica set_\):

```properties
spring.data.mongodb.uri=mongodb://user:secret@mongo1.example.com:12345.example.com:23456/test
```

Alternativamente, siempre que estés usando Mongo 2.x, especifica un _host/port_. Por ejemplo, podrías declarar lo siguiente en tu _application.properties_:

```
spring.data.mongodb.host=mongoserver
spring.data.mongodb.port=27017
```

> Nota
>
> _spring.data.mongodb.host \_y \_spring.data.mongodb.port_ no son soportados si estás usando el driver de Mongo 3.0 de Java. En tales casos, _spring.data.mongodb.uri_ deberían usarse para proveer toda la configuración.
>
> Tip
>
> Si_ \_no se especifica \_spring.data.mongodb.port_ se usa por defecto el 27017. Podrías simplemente borrar esta línea del ejemplo de arriba.
>
> Tip
>
> Si no estás usando Spring Data Mongo puedes inyectar los _beans_ de _com.mongodb.Mongo \_en lugar de usar \_MongoDbFactory._

También puedes declarar tu propia _MongoDbFactory_ o tu propio bean _Mongo_ si quieres tomar control completo al establecer la conexión a MongoDB.

#### MongoTemplate

Spring Data Mongo provee una clase MongoTemplate que es muy similar en su diseño a la _JdbcTemplate_ de Spring. Al igual que con _JdbcTemplate_ Spring Boot auto-configura por ti un bean para simplemente inyectarlo:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.stereotype.Component;

@Component
public class MyBean {

    private final MongoTemplate mongoTemplate;

    @Autowired
    public MyBean(MongoTemplate mongoTemplate) {
        this.mongoTemplate = mongoTemplate;
    }

    // ...
}
```

Para detalles completos consulta el Javadoc de _MongoOperations._

#### Repositorios MongoDB de Spring Data

Spring Data incluye soporte de repositorios para MongoDB. Al igual que los repositorios JPA discutidos anteriormente, el principio básico es que estas consultas se construyen automáticamente basado en el nombre de los métodos.

De hecho, ambos Spring Data JPA y Spring Data MongoDB tienen en común la misma infraestructura; así podrías tomar el ejemplo de JPA visto anteriormente y, asumiendo que _City_ es ahora una clase de datos Mongo en lugar de una entidad JPA \(_JPA @Entity_\), funcionará de la misma manera.

```java
package com.example.myapp.domain;

import org.springframework.data.domain.*;
import org.springframework.data.repository.*;

public interface CityRepository extends Repository<City, Long> {
    Page<City> findAll(Pageable pageable);
    City findByNameAndCountryAllIgnoringCase(String name, String country);
}
```

> Tip
>
> Para detalles completo de Spring Data MongoDB, incluyendo sus tecnologías de mapeo de objetos, consulte su documentación de referencia.

#### Embedded Mongo

Spring Boot ofrece auto-configuración para Embedded Mongo \(Mongo embebido\). Para usarlo en tu aplicación Spring Boot añade una dependencia en _de.flapdoodle.embed:de.flapdoodle.embed.mongo_.

El puerto en que Mongo escuchará puede configurarse usando la propiedad _spring.data.mongodb.port_. Para usar un puerto libre aleatorio usa el valor de cero. El _MongoClient_ creado por _MongoAutoConfiguration_ se configurará automáticamente para usar el puerto asignado aleatoriamente.

> Nota
>
> Si no configuras un puerto personalizado, el soporte embebido usará un puerto aleatorio por defecto \(en lugar del 27017\)

Si tienes SLF4J en el classpath, la salida producida por Mongo será automáticamente dirigida a un _logger_ llamado _org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongo_.

Puedes declarar tus propios beans _IMongodConfig_ y _IRuntimeConfig_ para tomar el control de la configuración de las instancias de Mongo y el ruteo del _logging_.

### 30.3 Neo4j

### 30.6 Elasticsearch

Elasticsearch es un motor de búsqueda y análisis de código abierto, distribuido y en tiempo real. Spring Boot ofrece auto-configuracióno básica para Elasticsearch y abstracciones de éste provistas por Spring Data ElasticSearch. Hay un 'Starter' _spring-boot-starter-data-elasticsearch_ para la recolección de dependencias de una manera conveniente. Spring Boot también soporta Jest.

#### Conectando a Elasticsearch usando Jest

#### Conectando a Elasticsearch usando Spring Data

Puedes inyectar una instancia auto-configurada de _ElasticsearchTemplate_ o Elasticsearch _Client_ así como cualquier otro Spring Bean. Por defecto la instancia incrustará un servidor local en-memoria \(un _Node_ en términos de Elasticsearch\) y usará el directorio actual como el directorio _home_ para el servidor\_. \_En esta configuración, la primera cosa por hacer es decirle a Elasticsearch dónde almacenar sus archivos:

```properties
spring.data.elasticsearch.properties.path.home=/foo/bar
```

Alternativamente, puedes cambiar a un servidor remoto \(i.e. un _TransportClient_\) configurando _spring.data.elasticsearch.cluster-nodes_ a una lista separada por comas de 'host:port'.

```properties
spring.data.elasticsearch.cluster-nodes=localhost:9300
```

```java
@Component
public class MyBean {
    private ElasticsearchTemplate template;

    @Autowired
    public MyBean(ElasticsearchTemplate template) {
        this.template = template;
    }

    // ...

}
```

Si añades _@Bean_ de tu propio tipo, _ElasticsearchTemplate_ reemplazará el bean por defecto.

#### Repositorios Spring Data Elasticsearch

Spring Data incluye soporte para los repositorios de Elasticsearch. Al igual que con los repositorios discutidos anteriormente, el principio básico es que las consultas se construyen automáticamente basado en el nombre de los métodos.

De hecho, ambos Spring Data JPA y Spring Data Elasticsearch comparten la misma infraestructura; así que podrías tomar el ejemplo de JPA visto anteriormente y, asumiendo que _City \_es ahora una clase _@Document _en lugar de una entidad JPA \(\_JPA @Entity_\), funcionará de la misma manera.

> Tip
>
> Para detalles completos de Spring Data Elasticsearch, consulta su documentación de referencia.

## 31. Almacenamiento en caché

El Framework Spring provee soporte para añadir caché de manera transparente a una aplicación. En su núcleo, la abstracción aplica caché a los métodos, reduciendo así el número de ejecuciones basado en la información disponible en la caché. La lógica del almacenamiento en caché se aplica transparentemente, sin ninguna interferencia a quien invoca. Spring Boot auto-configura la infraestructura de caché siempre que el soporte se encuentre habilitado mediante la anotación _@EnableCaching_.

> Nota
>
> Para más detalles revisa la sección relevante de la referencia de Spring Framework.

En pocas palabras, añadir caché a una operación de tu servicio es tan sencillo como añadir la anotación relevante en su método:

```
import org.springframework.cache.annotation.Cacheable
import org.springframework.stereotype.Component;

@Component
public class MathService {

    @Cacheable("piDecimals")
    public int computePiDecimal(int i) {
        // ...
    }

}
```

Este ejemplo demuestra el uso del caché en una operación potencialmente costosa. Antes de invocar  el método _computePiDecimals_, la abstracción buscará una entrada en la caché de _piDecimals_ que coincida con el argumento _i_. Si se encuentra una entrada, el contenido en la caché se retorna inmediatamente a quien llama y el método no se invoca. En caso contrario, el método se invoca y la caché se actualiza antes de retornar el valor.

> Nota
>
> También puedes usar las anotaciones del estándar JSR-107 \(Cache\) de manera transparente. Sin embargo, recomendamos encarecidamente que no se mezclen ni emparejen.

Si no añades ninguna librería específica de caché, Spring Boot auto-configurará un SimpleProvider que utiliza mapas concurrentes en memoria. Cuando se requiere una caché \(i.e. _piDecimals_ en el ejemplo de arriba\), este proveedor la creará por ti al-vuelo \(_on-the-fly_\). Realmente no se recomienda utilizar el _simple provider \_para un ambiente de producción, pero es magnífico para iniciar y asegurarse que se comprenden las características. Una vez que decidas sobre el proveedor que utilizarás, por favor asegúrate de leer su documentación para descifrar cómo configurar los cachés que usa tu aplicación. Prácticamente todos los proveedores requieren que configures explícitamente cada caché que usarás en la aplicación.  Algunos ofrecen una manera de personalizar los cachés por defecto definidos por la propiedad \_spring.cache.cache-names_.

> Tip
>
> También es posible actualizar o desalojar datos de la caché de manera transparente.
>
> Nota
>
> Si estás usando la infraestructura de caché con beans que no son basados en interfaz, asegúrate de habilitar el atributo _proxyTargetClass_ de _@EnableCaching_.

### 31.1 Proveedores de caché soportados

La abstracción del caché no provee un almacenamiento real y depende de la abstracción materializada por las interfaces _org.springframework.cache.Cache_ y _org.springframework.cache.CacheManager._

Si no has definido un bean del tipo _CacheManager_ o un _CacheResolver_ llamado _cacheResolver_ \(consulta _CachingConfigurer_\), Spring Boot tratará de detectar los siguientes proveedores \(en este orden\):

* Generic
* JCache \(JSR-107\) \(EhCache 3, Hazelcast, Infinispan, etc\)
* EhCache 2.x
* Hazelcast
* Infinispan
* Couchbase
* Redis
* Caffeine
* Guava \(obsoleto\)
* Simple

> Tip
>
> También es posible forzar el proveedor de caché a usar mediante la propiedad _spring.cache.type_. Usa esta propiedad si necesitas **deshabilitar el caché en conjunto** en ciertos ambientes \(por ejemplo en pruebas\).

> Tip
>
> Usa el 'Starter' _spring-boot-starter-cache_ para añadir rápidamente dependencias de caché básico. El _starter_ trae _spring-context-support_: si estás añadiendo dependencias manualmente, debes incluir _spring-context-support_ a fin de utilizar el soporte para JCache, EhCache 2.x o Guava.

Si el _CacheManager_ es auto-configurado por Spring Boot, puedes ajustar más la configuración antes de que esté completamente inicializado al exponer un bean implementando la interfaz _CacheManagerCustomizer_. Lo siguiente establece una bandera para decir que los valores nulos deberían pasarse al mapa subyacente.

```
@Bean
public CacheManagerCustomizer<ConcurrentMapCacheManager> cacheManagerCustomizer() {
    return new CacheManagerCustomizer<ConcurrentMapCacheManager>() {
        @Override
        public void customize(ConcurrentMapCacheManager cacheManager) {
            cacheManager.setAllowNullValues(false);
        }
    };
}
```

> Nota
>
> En el ejemplo de arriba, se espera un _ConcurrentMapCacheManager_ auto-configurado. Si ese no es el caso \(ya sea que proporcionaste tu propia configuración o que se auto-configuró un proveedor de caché diferente\), el _customizer_ \(personalizador\) no se invocará en lo absoluto. Puedes tener tantos _customizers_ como desees y puedes también ordenarlos como siempre usando _@Order_ o Ordered_._
