# Tareas básicas de JHipster

## Creando una aplicación

Por favor revisa nuestro video tutorial sobre cómo crear una nueva aplicación de JHipster!

1. Inicio rápido
2. Preguntas que te hace al generar una aplicación
3. Opciones de línea de comandos
4. Tips

## Inicio rápido

Antes que nada, crea un directorio vacío en el que se creará la aplicación:

```bash
mkdir myapplication
```

Entra a ese directorio

```bash
cd myapplication/
```

Para generar tu aplicación, escribe:

```bash
jhipster
```

Responde las preguntas que hace el generador para crear una aplicación que se ajuste a tus necesidades. Esas opciones se describen en la siguiente sección.

Una vez que se generó la aplicación, puedes iniciarla utilizando Maven \(`./mvnw` en Linux/MacOS/Windows PowerShell, `mvnw` en Windows Cmd\) o Gradle \(`./gradle` en Linux/MacOS/Windows PowerShell, `gradle` en Windows Cmd\).

La aplicación estará disponible en [http://localhost:8080](http://localhost:8080)

**Importante:** si quieres tener recarga en vivo \("live reload"\) de tu código JavaScript/TypeScript, necesitarás ejecutar gulp \(para JavaScript/AngularJS 1\) o yarn start \(para TypeScript/Angular 2+\). Para más información puedes visitar la página Utilizando JHipster en desarrollo.

## Preguntas que hace el generador

Algunas preguntas cambian dependiendo de las opciones previas que eliges. Por ejemplo, no necesitarás configurar el caché de Hibernate si no seleccionaste una base de datos SQL.

### ¿Qué tipo de aplicación te gustaría crear?

El tipo de tu aplicación depende de si deseas utilizar una arquitectura de microservicios o no. Encuentra [aquí ](http://www.jhipster.tech/microservices-architecture/)una explicación completa sobre microservicios, si no estás segura utiliza la opción por defecto "Monolithic application" \(Aplicación monolítica\).

Puedes usar:

* _Monolithic application _\(Aplicación monolítica\): es un clásico, una aplicación 'unitalla'. Es más fácil usar y desarrollar y es nuestra recomendación por defecto.
* _Microservice application _\(Aplicación de microservicio\): en una arquitectura de microservicios, éste es el único de los servicios.
* _Microservice gateway_ \(Gateway de microservicio\): en una arquitectura de microservicios, éste es un servidor perimetral que 'rutea' y protege las peticiones.
* JHipster UAA server \[BETA\]: en una arquitectura de microservicios, éste es un servidor de autenticación OAuth2 que protege a los microservicios. Para más información consulta la [documentación JHipster UAA](http://www.jhipster.tech/using-uaa/).

### ¿Cuál es el nombre base de tu aplicación?

Éste es el nombre de tu aplicación.

### ¿Cuál es el nombre del paquete por defecto de tu aplicación?

Tu aplicación Java usará éste como el paquete raíz. Este valor lo almacena Yeoman así que la próxima vez que ejecutes el generador el último valor se convertirá en el valor por defecto. Por supuesto que puedes sobre-escribirlo proporcionando un nuevo valor.

### ¿Deseas usar el JHipster Registry para configurar, monitorear y escalar tu aplicación?

El JHipster Registry es una herramienta de código abierto que se usa para administrar tu aplicación en tiempo de ejecución.

Es requerida cuando se usa una arquitectura de microservicios \(ésta es la razón por la que esta pregunta se hace solo cuando se genera una aplicación monolítica\).

### ¿Qué tipo de autenticación te gustaría usar?

Esta pregunta no se hace cuando seleccionas el JHipster Registry, ya que requiere el uso de la autenticación por JWT.

Puedes usar:

* JSON Web Token \(JWT\), es la opción por defecto
* Un mecanismo clásico de autenticación basada en sesión, como estamos acostumbrados a hacerlo en Java \(así es como la mayoría de la gente usa Spring Security\). Puedes usar esta opción con Spring Social, el cual te permitirá usar el "social login" \(como Google, Facebook, Twitter\): éste se configura mediante el soporte de Spring Social de Spring Boot. 
* Un mecanismo de autenticación OAuth2.0 \(JHipster proporciona el código del servidor OAuth2 necesario y las tablas de bases de datos\).

Los enfoques OAuth2.0 y JWT permiten utilizar una arquitectura de aplicación \(_stateless_\) 'sin estado' \(éstos no se basan en la sesión HTTP\). Puedes encontrar más información en la página de [protegiendo tu aplicación](http://www.jhipster.tech/security/).

### ¿Qué tipo de base de datos te gustaría utilizar?

Puedes elegir entre:

* Ninguna base de datos \(sólo disponible cuando se usa una aplicación de microservicio\).
* Una base de datos SQL \(H2, MySQL, MariaDB, PostgreSQL, MSSQL, Oracle\), la cual accederás con Spring Data JPA. 
* MongoDB
* Cassandra

### ¿Cuál base de datos te gustaría utilizar para producción?

Ésta es la base de datos que usará tu perfil de "producción". Para configurarlo, por favor modifica tu archivo `src/main/resources/config/application-prod.yml`.

Si deseas usar Oracle, necesitarás instalar manualmente el driver JDBC de Oracle.

### ¿Cuál base de datos te gustaría utilizar para desarrollo?

Ésta es la base de datos que utilizarás con tu perfil de "desarrollo". Puedes usar:

* H2, corriendo en memoria. Ésta es la manera más fácil de usar JHipster pero tus datos se perderán cuando reinices el servidor.
* H2, con datos almacenados en disco. Actualmente se encuentra en prueba BETA \(y no funciona en Windows\), pero eventualmente sería una mejor opción que el H2 corriendo en memoria, de esta manera no perderás tus datos cuando la aplicación se reinicie. 
* La misma base de datos que elegiste para producción: es un poco más complicada de configurar pero al final sería mejor trabajar en la misma base de datos que usarás en producción. También es la mejor manera de usar liquibase-hibernate como se describe en la [guía de desarrollo.](http://www.jhipster.tech/development/)

Para configurarla, por favor modifica tu archivo `src/main/resources/config/application-dev.yml`.

### ¿Deseas usar caché de segundo nivel de Hibernate?

Hibernate es el proveedor JPA que usa JHipster. Por razones de rendimiento, recomendamos encarecidamente que uses un caché y que lo ajustes de acuerdo a tus necesidades. Si eliges hacerlo, puedes usar también ehcache \(caché local\) o Hazelcast \(caché distribuido, para uso en un ambiente de clúster\).

### ¿Te gustaría usar Maven o Gradle?

Puedes construir tu aplicación de Java generada ya sea con Maven o con Gradle. Maven es más estable y más maduro. Gradle es más flexible, fácil de extender y más _hype_.

### ¿Qué otras tecnologías te gustaría usar?

Ésta es una pregunta multi-respuesta, para agregar una o más tecnologías a la aplicación. Las tecnologías disponibles son:

#### **Social login \(Google, Facebook, Twitter\)**

Esta opción solo se encuentra disponible si seleccionaste una base de datos SQL o una base de datos MongoDB. Añade soporte de Spring Social a JHipster, de esta manera los usuarios finales se pueden autenticar utilizando su cuenta de Google, Facebook o Twitter.

#### _API first development_ utilizando swagger-codegen

Esta opción te permite hacer un _API first development \_para tu aplicación integrando \_Swagger-Codegen_ en el _build _\(en este tipo de desarrollo, en lugar de generar la documentación a partir del código, necesitas escribir primero la especificación y luego generar código a partir de ésta\).

#### Motor de búsqueda usando Elasticsearch

Elasticsearch se configura utilizando Spring Data ElasticSearch. Encuentra más información en nuestra [guía de Elasticsearch](http://www.jhipster.tech/using-elasticsearch/).

#### Sesiones HTTP en clúster utilizando Hazelcast

Por defecto, JHipster utiliza sesiones HTTP solo para almacenar información de autenticación y autorización de Spring Security. Por supuesto, puedes elegir poner más datos en tus sesiones HTTP. El uso de sesiones HTTP provocará problemas si estás ejecutando en un clúster, especialmente si no utilizas un balanceador de cargas con "sticky sessions". Si deseas replicar tus sesiones dentro de un clúster, elige esta opción para tener configurado Hazelcast.

#### WebSockets usando Spring Websocket

Los websockets se pueden habilitar usando Spring Websocket. También proporcionamos una muestra completa para mostrarte cómo usar el framework de manera eficiente.

#### Mensajes asíncronos usando Apache Kafka

Usa Apache Kafka como un agente de mensajes de publicación/suscripción.

### ¿Cuál _Framework_ te gustaría utilizar para el cliente?

El _framework_ para usar del lado del cliente.

Puedes usar:

* Angular versión 4+
* AngularJS versión 1.x \(próximamente será obsoleto\)

### ¿Te gustaría utilizar el pre-procesador de hojas de estilo _LibSass_ para tus CSS?

Node-Sass es una magnífica solución para simplificar el diseño CSS. Para usarla de manera eficiente, necesitas ejecutar un servidor Gulp, el cual será configurado automáticamente.

### ¿Te gustaría habilitar el soporte de internacionalización?

Por defecto JHipster proporciona un excelente soporte de internacionalización. ya sea del lado del cliente o del servidor. Sin embargo, la internacionalización añade una pequeña carga, y es un poco más compleja de manejar, así que puedes elegir no instalar esta característica.

### ¿Cuál _framework_ de pruebas te gustaría utilizar?

Por defecto JHipster proporciona pruebas unitarias y de integración \(usando el soporte de JUnit de Spring\). Opcionalmente, también puedes añadir soporte para:

* Pruebas de rendimiento usando Gatling
* Pruebas de comportamiento usando Cucumber
* Pruebas de integración de Angular con Protractor

Puedes encontrar más información en nuestra [guía de Ejecución de pruebas.](http://www.jhipster.tech/running-tests/)

### ¿Te gustaría instalar otros generadores del JHipster Marketplace?

El JHipster Marketplace es donde puedes instalar módulos adicionales, escritos por otros desarrolladores, para añadir a tu proyecto características no oficiales.

## Opciones de línea de comandos

También puedes ejecutar JHipster con algunas opciones opcionales de línea de comandos. Puedes encontrar referencias de estas opciones escribiendo `jhipster app --help`.

Éstas son las opciones que puedes pasar:

* `--help` - Muestra las opciones del generador y el uso
* `--skip-cache` - No recuerda las respuestas rápidas \(Por defecto en _false_\).
* `--skip-install` - No instala dependencias de manera automática \(Por defecto en _false_\)
* `--skip-client` - Omite la generación de la aplicación del lado del cliente \(_front-end_\), de esta manera solo se genera el código del _back-end_ \(Por defecto en _false_\). Esto tiene el mismo efecto que ejecutar el sub-generador del servidor con jhipster server.
* `--skip-server` - Omite la generación de la aplicación del lado del servidor, de esta manera solo se genera el código del lado del _front-end _\(Por defecto en _false_\). Esto tiene el mismo efecto que ejecutar el sub-generador del cliente con jhipster client. 
* `--skip-user-management` - Omite la generación de la administración de usuarios, tanto en el _front _ como en el _back-end_. \(Por defecto en _false_\).
* `--i18n` - Habilita o deshabilita i18n durante la generación del _front-end_, en caso contrario no tiene efecto \(Por defecto en _true_\).
* `--auth` - Especifica el tipo de autenticación cuando se omite la generación del _back-end_, en caso contrario no tiene efecto pero es requerido cuando se usa skip-server.
* `--db` - Especifica la base de datos cuando se omite la generación del _back-end_, en otro caso no tiene efecto pero es obligatorio cuando se usa skip-server
* `--with-entitites` - Regenera las entidades existentes si ya habían sido generadas \(usando su configuración en la carpeta .jhipster\) \(Por defecto en _false_\).
* `--skip-checks` - Omite la revisión de las herramientas requeridas \(Por defecto en _false_\).
* `--jhi-prefix` - Añade prefijos a los nombres de los servicios, componentes y estados/rutas \(Por defecto: jhi\).
* `--npm` - Usa NPM en lugar de Yarn \(Por defecto en _false_\). 

> Tips
>
> Si eres un usuario avanzado puedes utilizar nuestros sub-generadores de cliente y servidor ejecutando `jhipster client --[options]` y `jhipster server --[options]`. Para ver todas las opciones que se pueden usar ejecuta los subgeneradores anteriores con la bandera `--help`.
>
> También puedes usar las opciones de línea de comandos de Yeoman, como --force para sobre escribir automáticamente los archivos existentes. Si quieres regenerar tu aplicación completa, incluyendo sus entidades, puedes ejecutar `jhipster --force --with-entities`.

#### 



