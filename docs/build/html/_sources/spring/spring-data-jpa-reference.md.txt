# Spring Data JPA - Documentación de referencia

[Documento original](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#dependencies.spring-framework)

## 3. Dependencias

Debido a las diferentes fechas de creación de los módulos individuales de Spring Data, la mayoría de ellos llevan números diferentes de versiones mayores y menores. La manera más fácil de encontrar compatibles es confiando en el _Spring Data Release Train BOM_. En un proyecto Maven declararías esta dependencia en la sección &lt;dependencyManagement/&gt;  de tu POM:

**Ejemplo 1: Usando el **_**release train **_**de Spring Data**

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-releasetrain</artifactId>
      <version>${release-train}</version>
      <scope>import</scope>
      <type>pom</type>
    </dependency>
  </dependencies>
</dependencyManagement>
```

La versión actual del _release train_ es _Ingalls-SR6_. Los nombres del _train \_se encuentran disponibles y ordenados alfabéticamente _[_aquí_](https://github.com/spring-projects/spring-data-commons/wiki/Release-planning)_. El nombre de la versión sigue el siguiente patrón: _`${name}-${release}`_ donde \_release_ puede ser uno de los siguientes:

* BUILD-SNAPSHOT - snapshots actuales
* M1, M2 etc. - milestones \(hitos\)
* RC1, RC2 etc. - candidatos de lanzamiento
* RELEASE - GA release \(lanzamiento de disponibilidad general\)
* SR1, SR2 etc. - lanzamientos de servicio

Un ejemplo de trabajo de uso de BOMs \(_Bills of materials_; Lista de materiales\) se encuentra en nuestro [repositorio de ejemplos de Spring Data](https://github.com/spring-projects/spring-data-examples/tree/master/bom). Si eso está en su lugar declara, sin especificar ninguna versión, los módulos de Spring Data que te gustaría usar en el bloque _&lt;dependencies /&gt;_.

**Ejemplo 2: Declarando una dependencia a un módulo de Spring Data**

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
  </dependency>
<dependencies>
```

## 3.1 Administración de dependencias con Spring Boot

Spring Boot selecciona por ti una versión muy reciente de los módulos de Spring Data. No obstante, en caso que desees actualizar a una nueva versión, simplemente configura la propiedad _spring-data-releasetrain.version_ al nombre del tren e iteración que te gustaría usar.

## 3.2 El Framework Spring

La versión actual de los módulos de Spring Data requiere la versión 4.3.10.RELEASE de Spring Framework o una versión mayor. Los módulos también podrían funncionar con una versión _bugfix_ anterior de esa versión menor. Sin embargo, es altamente recomendable utilizar la versión más reciente dentro de esa generación.

## 4. Trabajando con Repositorios Spring Data

El propósito de la abstracción del repositorio Spring Data es reducir significativamente la cantidad de código repetitivo \(_boilerplate code_\) requerido para implementar capas de acceso de datos para varios almacenes de persistencia.

> La documentación de repositorio Spring Data y tu módulo
>
> ...

## 4.1 Conceptos básicos

La interfaz central en la abstracción del repositorio de Spring Data es _Repository_ \(probablemente no es sorpresa\). Ésta toma la clase dominio para administrara tanto el tipo de id de la clase dominio como el tipo de argumentos. Esta interfaz actúa principalmente como una interfaz de marcado para capturar los tipos con los que hay que trabajar y te ayuda a descubrir interfaces que extienden de ésta. El _CrudRepository_ proporciona sofisticada funcionalidad CRUD para la clase entidad que se está gestionando.

**Ejemplo 3: Interfaz **_**CrudRepository**_

```
public interface CrudRepository<T, ID extends Serializable>
    extends Repository<T, ID> {

    <S extends T> S save(S entity); // Guarda la entidad

    T findOne(ID primaryKey);       // Retorna la entidad identificada por ese id

    Iterable<T> findAll();          // Retorna todas las entidades

    Long count();                   // Retorna el número de entidades

    void delete(T entity);          // Elimina la entidad proporcionada

    boolean exists(ID primaryKey);  // Indica si existe una entidad con tal id

    // … more functionality omitted.
}
```

> También proporcionamos abstracciones de tecnología específica como por ejemplo _**JpaRepository **\_o _**MongoRepository**_. Esas interfaces extienden de _**CrudRepository**\_ y exponen las capacidades de la tecnología de la persistencia subyacente además de las interfaces de tecnología-agnóstica que son más bien genéricas como por ejemplo CrudRepository.

Encima del _CrudRepository_ existe una abstracción _PagingAndSortingRepository_ que añade métodos adicionales para facilitar el acceso paginado a entidades:

**Ejemplo 4. PagingAndSortingRepository**

```java
public interface PagingAndSortingRepository<T, ID extends Serializable>
  extends CrudRepository<T, ID> {

  Iterable<T> findAll(Sort sort);

  Page<T> findAll(Pageable pageable);
}
```

Para acceder a la segunda página de un _User_ por una página de tamaño 20 podrías simplemente hacer algo como esto:

```java
PagingAndSortingRepository<User, Long> repository = // … get access to a bean
Page<User> users = repository.findAll(new PageRequest(1, 20));
```

Además de los métodos de consulta, está disponible la derivación de consultas para los _queries_ _count \_y \_delete_.

**Ejemplo 5. Consulta **_**Count**_** derivada**

```java
public interface UserRepository extends CrudRepository<User, Long> {

  Long countByLastname(String lastname);
}
```

**Ejemplo 6. Consulta **_**Delete **_**derivada**

```java
public interface UserRepository extends CrudRepository<User, Long> {

  Long deleteByLastname(String lastname);

  List<User> removeByLastname(String lastname);

}
```

## 4.2 Métodos de consulta

Los repositorios estándar de funcionalidad CRUD usualmente tienen consultas sobre el subyacente almacén de datos. Con Spring Data, declarar esas consultas se convierte en un proceso de cuatro pasos:

1. Declara una interfaz que extienda de Repository o una de sus subinterfaces y escribe la clase de dominio y el tipo del ID que manejará.
   ```
   interface PersonRepository extends Repository<Person,Long>{…}
   ```
2. Declara los métodos de consulta en la interfaz.
   ```java
   interface PersonRepository extends Repository<Person, Long> {
     List<Person> findByLastname(String lastname);
   }
   ```
3. Prepara a Spring para crear instancias proxy para esas interfaces. Puede ser mediante [JavaConfig](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.create-instances.java-config):

   ```java
   import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

   @EnableJpaRepositories
   class Config {}
   ```

o mediante [configuración XML:](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.create-instances)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:jpa="http://www.springframework.org/schema/data/jpa"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
     http://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/data/jpa
     http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

   <jpa:repositories base-package="com.acme.repositories"/>

</beans>
```

En este ejemplo se usa el _namespace_ JPA. Si estás usando la abstracción de repositorio para cualquier otro almacén, necesitas cambiar este a la declaración apropiada del _namespace_ de tu módulo de almacenamiento el cual debería estar intercambiando _jpa_ en favor de, por ejemplo, _mongodb._

Nota, también, que la variante JavaConfig no configura explícitamente un paquete como el paquete de la clase anotada es usado por defecto. To customize the package to scan use one of the `basePackage…`attribute of the data-store specific repository `@Enable…`-annotation.

1. Inyecta la instancia del repositorio y úsala.

```java
public class SomeClient {

  @Autowired
  private PersonRepository repository;

  public void doSomething() {
    List<Person> persons = repository.findByLastname("Matthews");
  }
}
```

La siguiente sección explica cada paso en detalle.

## 4.3 Definiendo interfaces de repositorios

Como primer paso puedes definir una interfaz de repositorio de una clase de dominio específica. La interfaz debe extender de Repository y escribirle la clase de dominio y el tipo de dato del ID. Si deseas exponer métodos CRUD para ese tipo de dominio, extiende de CrudRepository en lugar de Repository.

### 4.3.1 Definición de grano fino de repositorio

Normalmente, tu interfaz de repositorio extenderá de _Repository_, _CrudRepository_ o _PagingAndSortingRepository_. Alternativamente, si no quieres extender de las interfaces de Spring Data, también puedes anotar tu interfaz de repositorio con _@RepositoryDefinition_. Al extender de _CrudRepository_ se expone un completo conjunto de métodos para manipular tus entidades. Si prefieres ser selectivo sobre los métodos que se están exponiendo, simplemente copia de _CrudRepository_ los que quieres exponer y pégalos en tu repositorio de dominio.

> Esto permite definir tus propias abstracciones sobre las funcionalidades que proporcionan los Repositorios de Spring Data.

**Ejemplo 8: Exponiendo métodos CRUD de manera selectiva**

```
@NoRepositoryBean
interface MyBaseRepository<T, ID extends Serializable> extends Repository<T, ID> {

  T findOne(ID id);

  T save(T entity);
}

interface UserRepository extends MyBaseRepository<User, Long> {
  User findByEmailAddress(EmailAddress emailAddress);
}
```

En este primer paso se define una interfaz de base común para todos tus repositorios de domino y se expone _findOne\(...\)_ así como _save\(...\)_. Estos métodos se enviarán a la implementación base de repositorio del almacén de tu elección provista por Spring Data, por ejemplo en el caso de JPA sería _SimpleJpaRepository, \_porque coinciden con las firmas del método en \_CrudRepository_. Así que el _UserRepository_ ahora será capaz de guardar usuarios y encontrarlos por id, así como definir una consulta para encontrar _Users_ por su _e-mail_.

> Nota que la interfaz de repositorio intermedia se encuentra anotada con _@NoRepositoryBean_. Asegúrate de añadir esa anotación a todos las interfaces de repositorio de las que no deseas que Spring Data cree instancias en tiempo de ejecución.

### 4.3.2 Usando Repositorios con múltiples módulos de Spring Data

Usar un único módulo de Spring Data en tu aplicación hace las cosas sencillas, todas las interfaces de repositorio en el alcance definido se encuentran ligadas al módulo de Spring Data. A veces las aplicaciones requieren usar más de un módulo de Spring Data. En tal caso, se requiere que una definición de repositorio distinga entre tecnologías de persistencia. Spring Data entra en modo de configuración estricta de repositorio porque detecta múltiples fábricas de repositorio en el _class path_. La configuración estricta requiere detalles en el repositorio o la clase de dominio para decidir sobre a qué módulo de Spring Data ligar a la definición del repositorio:

1. Si la definición del repositorio **extiende de un repositorio específico de módulo, **entonces es un candidato válido para ese particular módulo de Spring Data.
2. Si la clase de dominio se encuentra **anotada con el tipo de anotación específica de módulo**, entonces es un candidato válido para ese particular módulo de Spring Data. Los módulos de Spring Data aceptan cualquier anotación de terceros \(tales como entidades de JPA; _JPA's @Entity_\) o proporcionan anotaciones propias tales como _@Document_ para Spring Data MongoDB/Spring Data Elasticsearch.

**Ejemplo 8. Definiciones de repositorios utilizando Interfaces específicas de módulos**

```java
interface MyRepository extends JpaRepository<User, Long> { }

@NoRepositoryBean
interface MyBaseRepository<T, ID extends Serializable> extends JpaRepository<T, ID> {
  …
}

interface UserRepository extends MyBaseRepository<User, Long> {
  …
}
```

_MyRepository_ y _UserRepository_ extienden de _JpaRepository_ en su jerarquía de tipos. Éstos son candidatos válidos para el módulo Spring Data JPA.

**Ejemplo 9. Definiciones de Repositorios utilizando Interfaces genéricas**

```
interface AmbiguousRepository extends Repository<User, Long> {
 …
}

@NoRepositoryBean
interface MyBaseRepository<T, ID extends Serializable> extends CrudRepository<T, ID> {
  …
}

interface AmbiguousUserRepository extends MyBaseRepository<User, Long> {
  …
}
```

_AmbiguosRepository \_y \_AmbigousUserRepository_ extienden solo de \_Repository \_y \_CrudRepository \_en su jerarquía de tipos. Aunque está perfectamente bien utilizar un único módulo de Spring Data, múltiples módulos no pueden distinguir a cuál módulo en particular deben enlazarse estos repositorios.

**Ejemplo 10. Definiciones de Repositorios utilizando Clases de Dominio con Anotaciones**

```java
interface PersonRepository extends Repository<Person, Long> {
 …
}

@Entity
public class Person {
  …
}

interface UserRepository extends Repository<User, Long> {
 …
}

@Document
public class User {
  …
}
```

_PersonRepository \_hace referencia a \_Person_ el cual se encuentra anotado con la anotación JPA _@Entity_ así que este repositorio pertenece claramente a Spring Data JPA. El _UserRepository_ utiliza a _User_ anotado con la anotación _@Document_ de Spring Data MongoDB.

**Ejemplo 11. Definiciones de Repositorios utilizando Clases de Dominio con Anotaciones mixtas**

```java
interface JpaPersonRepository extends Repository<Person, Long> {
 …
}

interface MongoDBPersonRepository extends Repository<Person, Long> {
 …
}

@Entity
@Document
public class Person {
  …
}
```

Este ejemplo muestra una clase de dominio usando tanto anotaciones JPA como anotaciones Spring Data MongoDB. Ésta define dos repositorios, _JpaPersonRepository_ y _MongoDBPersonRepository_. Uno está destinado para uso con JPA y otro para uso con MongoDB. Spring Data no es capaz de distinguir a los repositorios lo que conduce a un comportamiento indefinido.

[Los detalles del tipo de repositorio](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.multiple-modules.types) y la [identificación de anotaciones de clases de dominio](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.multiple-modules.annotations) se utilizan para la configuración estricta del repositorio identificando así candidatos de repositorio para un módulo particular de Spring Data. Es posible utilizar múltiples anotaciones de persistencia de tecnologías específicas en el mismo tipo de dominio a través de múltiples tecnologías de persistencia, pero entonces Spring Data no será capaz de determinar un único módulo para enlazar el repositorio.

La manera anterior de distinguir repositorios es determinar el alcance de los paquetes base. Los paquetes base definen los puntos de inicio para el escaneo de definiciones de interfaces de repositorio lo cual implica tener definiciones  de repositorio ubicadas en los paquetes apropiados. Por defecto, la configuración por anotaciones utiliza el paquete de la clase de configuración. En la configuración por XML el paquete base es requerido.

**Ejemplo 12. Configuración de paquetes base por medio de anotaciones**

```java
@EnableJpaRepositories(basePackages = "com.acme.repositories.jpa")
@EnableMongoRepositories(basePackages = "com.acme.repositories.mongo")
interface Configuration { }
```

## 4.4 Definiendo métodos de consulta

El repositorio proxy tiene dos maneras de derivar una consulta de almacén específico basado en el nombre del método. Puede derivar la consulta directamente del nombre del método o usando una consulta definida manualmente. La opciones disponibles dependen del almacén real. Sin embargo, tiene que ser una estrategia que decida cuál consulta en concreto se crea. Revisemos las opciones disponibles.

### 4.4.1 Estrategias de búsqueda de consultas

Las siguientes estrategias están disponibles para la infraestructura del repositorio para resolver la consulta. Puedes configurar la estrategia en el _namespace_ mediante el atributo _query-lookup-strategy_ en el caso de configuración por XML o en el caso de configuración por Java mediante el atributo _queryLookupStrategy_ en la anotaciónEnable${store}Repositores. Algunas estrategias podrían no estar soportadas para _datastores_ particulares.

* CREATE trata de construir con _query_ específico del almacén a partir del nombre del método. El enfoque general es remover del nombre del método un conjunto de prefijos ya conocidos  y analizar el resto del método. Lee más sobre la construcción de _queries_ en [**Creación de queries**.](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.query-creation)



* USE\_DECLARED\_QUERY trata de encontrar un _query_ definido y en caso de no encontrarlo lanzará una excepción. El _query_ puede definirse con una anotación en algún lugar o puede ser declarado por otros medios. Consulta la documentación del almacén específico para encontrar opciones disponibles para dicho almacén. Si la infraestructura del repositorio no encuentra un _query_ declarado para el método en tiempo de arranque éste falla.



* CREATE\_IF\_NOT\_FOUND \(por defecto\) combina _CREATE_ y _USE\_DECLARED\_QUERY_. Primero busca un _query_ declarado y si no encuentra ninguno, crea un _query _personalizado basándose en el nombre del método. Por defecto esta es la estrategia de búsqueda y por tanto se usará si no configuras nada explícitamente. Esto permite la definición rápida de la _queries_ mediante nombres de métodos, pero también la personalización de estos _queries _mediante la introducción de _queries_ declarados según sea necesario.

### 4.4.2 Creación de _queries_

El mecanismo de construcción de _queries_ integrado en la infraestructura de Spring Data repository es útil para construir _queries_ restringidos sobre las entidades del repositorio. El mecanismo quita del método los prefijos _find...By_, _read...By_, _query...By_, _count...By _y _get...By_ y analiza el resto. La cláusula que se introduzca puede contener otras expresiones como _Distinct_ para establecer una bandera de 'distintos' en el _query_ que se crea. Sin embargo, el primer _By_ actúa como delimitador para indicar el inicio del criterio real. En un nivel muy básico puedes definir condiciones o propiedades de entidad y concatenarlas con _And _y _Or_.

**Ejemplo 13. Creación de **_**queries**_** a partir de los nombres de los métodos**

```java
public interface PersonRepository extends Repository<User, Long> {

  List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);

  // Enables the distinct flag for the query
  List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
  List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);

  // Enabling ignoring case for an individual property
  List<Person> findByLastnameIgnoreCase(String lastname);
  // Enabling ignoring case for all suitable properties
  List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);

  // Enabling static ORDER BY for a query
  List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
  List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

El resultado real del análisis gramatical de los métodos depende del almacenamiento de persistencia para el cual creaste el _query_. Sin embargo, hay algunas cosas generales que notar:

* Las expresiones usualmente son propiedades concatenadas mediante operadores. Puedes combinar las expresiones de propiedad con _AND _y _OR_. También tienes soporte para operadores como _Between_, _LessThan_, _GreaterThan_, _Like_ para las expresiones de propiedad. Los operadores soportados pueden variar por _datastore_, así que consulta la parte apropiada de tu documentación de referencia.
* El analizador de métodos soporta el establecimiento de una bandera para propiedades individuales \(por ejemplo, _findByLastnameAndFirstnameAllIgnoreCase\(...\)_\). El _IgnoreCase_ puede o no estar soportado dependiendo del almacén, así que consulta las secciones relevantes en la documentación de referencia para el método de consulta_ _específico del almacén.
* Puedes aplicar ordenamiento estático añadiendo una cláusula _OrderBy_ al método de consulta que hace referencia a una propiedad y proporcionar una dirección al ordenamiento \(_Asc_ o _Desc_\). Para crear un método de consulta que soporte ordenamiento dinámico, consulta _Manejo parámetros especiales_.

### 4.4.3 Expresiones de propiedad

Las expresiones de propiedad pueden referirse solo a propiedades directas de la entidad que se administra, como se mostró en el ejemplo anterior. En la creación del _query _te aseguras que la propiedad que escribes en el nombre del método es una propiedad de la clase de dominio gestionada. Sin embargo, también puedes definir restricciones anotando propiedades anidadas. Supón que una _Person_ tiene una _Address_ con un _ZipCode_. En ese caso un método con el nombre:

```java
List<Person> findByAddressZipCode(ZipCode zipCode);
```

crea la propiedad transversal _x.address.zipCode_. El algoritmo de resolución comienza interpretando la parte entera \(_AddressZipCode_\) como la propiedad y verifica que la clase de dominio tenga una propiedad con ese nombre \(en minúsculas\). Si el algoritmo encuentra la propiedad entones la usa. Si no, el algoritmo divide la fuente en partes 'Camel Case' tomando el lado derecho como la cabeza y el resto como la cola y trata de encontrar la propiedad correspondiente, en nuestro ejemplo, _AddressZip _y _Code_. Si el algoritmo encuentra una propiedad con tal cabeza toma la cola y continúa construyendo el árbol a partir de allí, dividiendo la cola en la manera antes descrita. Si la primera división no coincide, el algoritmo mueve el punto de división a la izquierda \(_Address, ZipCode_\) y continúa.

Aunque esto debería funcionar para la mayoría de los casos, es posible que el algoritmo seleccione la propiedad equivocada. Supón que la clase _Person_ tiene también una propiedad _AddressZip_. El algoritmo coincidiría en la primera ronda de división y esencialmente elegiría la propiedad equivocada y finalmente fallaría \(puesto que el tipo _addressZip_ probablemente no tiene la propiedad _code_\).

Para resolver esta ambigüedad puedes usar un guión bajo \(_\__\) en el nombre de tu método para definir manualmente puntos transversales. Así que el nombre de nuestro método sería:

```java
List<Person> findByAddress_ZipCode(ZipCode zipCode);
```

Debido a que definimos al guión bajo como un caracter reservado recomendamos encarecidamente seguir las convenciones de nomenclatura estándar de Java \(por ejemplo usar Camel Case en los nombres de las propiedades y **no **guión bajo\).

### 4.4.4 Manejo de parámetros especiales

Para manejar parámetros especiales en tu _query_ simplemente define parámetros de método como ya se ha visto en los ejemplos de arriba. Además de que la infraestructura reconocerá ciertos tipos específicos tales como _Pageable_ y _Sort_ para aplicar paginación y ordenamiento a tus _queries_ de manera dinámica.

**Ejemplo 14. Usando **_**Pageable**_**, **_**Slice**_** y Sort**_** **_**en los métodos de consulta**

```java
Page<User> findByLastname(String lastname, Pageable pageable);

Slice<User> findByLastname(String lastname, Pageable pageable);

List<User> findByLastname(String lastname, Sort sort);

List<User> findByLastname(String lastname, Pageable pageable);
```

El primer método permite que le pases una instancia de _org.springframework.data.domain.Pageable_ al método de consulta para añadir dinámicamente paginación a tu _query_ definido estáticamente. Una _Page_ sabe el número total de elementos y páginas disponibles. Lo hace por la infraestructura que activa una consulta de recuento para calcular el número total.
 Aunque esto podría ser costoso dependiendo del almacén que se utilice, en su lugar puede utilizarse _Slice_. Un _Slice_ solo sabe si hay otro _Slice_ disponible lo cual podría ser suficiente cuando se recorre un conjunto de resultados grande.

Las opciones de ordenamiento también se manejan mediante la instancia _Pageable_. Si solo necesitas ordenar, simplemente añade un parámetro _org.springframework.data.domain.Sort_ a tu método. Como puedes ver, también es posible retornar una _List_. En este caso los metadatos requeridos para construir la instancia _Page_ no serán creados \(lo cual a su vez significa que no se ejecuta el _query_ adicional que hace el recuento\) sino simplemente restringe que el _query_ busque solo el rango proporcionado de entidades.

> Para descubrir cuántas páginas se obtienen en total tienes que ejecutar una consulta de recuento adicional. Por defecto esta consulta será derivada del _query_ que se ejecuta.

### 4.4.5 Limitando los resultados de los _queries_

Se puede limitar el resultado de los métodos de consulta mediante las palabras clave \(_keywords_\) _first_ o _top_, los cuales se pueden usar de manera intercambiable. De manera opcional se puede añadir al principio un valor numérico para especificar el tamaño máximo del resultado a retornar. Si se omite este número se asume un tamaño de resultado de 1.

**Ejemplo 15. Limitando el tamaño del resultado de un **_**query**_** con **_**Top **_**y **_**First**_

```
User findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

Slice<User> findTop3ByLastname(String lastname, Sort sort);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```

Al limitar las expresiones también se puede utilizar la palabra clave _Distinct_. En el caso de los _queries_ que limitan el resultado a una sola instancia, también se puede 'envolver' el resultado en un _Optional_.

Si la paginación o el _slicing_ se aplica a un _query_ de paginación limitado \(y al cálculo del número de páginas disponibles\) entonces éstos se aplican dentro del resultado limitado.

> Nota que la limitación de resultados en combinación con el ordenamiento dinámico mediante un parámetro _Sort_ permite expresar métodos de consulta para los 'K' elementos más pequeños así como también a los 'K' elementos más grandes.

### 4.4.6 _Streaming_ de resultados de _queries_

Los resultados de los métodos de consulta se pueden procesar incrementalmente utilizando un _Stream&lt;T&gt;_ de Java 8 como tipo de retorno. En lugar de simplemente 'envolver' \(_wrapping_\) los resultados de la consulta en un _Stream_, se utilizan los métodos específicos del _data store _para realizar el _streaming_.

**Ejemplo 16. **_**Streaming**_** de los resultados de un **_**query**_** con **_**Stream&lt;T&gt; **_**de Java 8**

```java
@Query("select u from User u")
Stream<User> findAllByCustomQueryAndStream();

Stream<User> readAllByFirstnameNotNull();

@Query("select u from User u")
Stream<User> streamAllPaged(Pageable pageable);
```

> Un _Stream_ 'envuelve'  potencialmente los recursos específicos del _data store_ subyacente y debe por lo tanto cerrarse después de usarse. También puedes cerrar el _Stream_ usando el método _close\(\)_ o usando el bloque _try-with-resources_ de Java 7.

**Ejemplo 17. Trabajando con un resultado **_**Stream&lt;T&gt;**_** en un bloque **_**try-with-resources**_

```java
try (Stream<User> stream = repository.findAllByCustomQueryAndStream()) {
  stream.forEach(…);
}
```

> Actualmente no todos los módulos de Spring Data soportan _Stream&lt;T&gt; _como tipo de retorno.

### 4.4.7 Resultados de _queries _asíncronos

Las consultas a los repositorios se pueden ejecutar de manera asíncrona utilizando la capacidad de ejecución asíncrona de métodos de Spring. Esto significa que el método retornará inmediatamente después de la invocación y realmente la ejecución del _query_ ocurrirá en una tarea que se envía al Ejecutor de Tareas de Spring \(_Spring Task Executor_\).

```java
@Async
Future<User> findByFirstname(String firstname);   // Usa a java.util.concurrent.Future como tipo de retorno            

@Async
CompletableFuture<User> findOneByFirstname(String firstname); // Usa a java.util.concurrent.CompletableFuture de Java 8 como tipo de retorno

@Async
ListenableFuture<User> findOneByLastname(String lastname);// Usa a org.springframework.util.concurrent.ListenableFuture como tipo de retorno.
```
