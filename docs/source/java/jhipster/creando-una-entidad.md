# Creando una entidad

_Por favor revisa nuestro _[_video tutorial_](http://www.jhipster.tech/video-tutorial/)_ sobre cómo crear una nueva aplicación JHipster!_

**Importante:** si quieres tener recarga en vivo \("live reload"\) de tu código JavaScript/TypeScript, necesitarás ejecutar gulp \(para JavaScript/AngularJS 1\) o yarn start \(para TypeScript/Angular 2+\). Para más información puedes visitar la página Utilizando JHipster en desarrollo.

## Introducción

Una vez creada tu aplicación, querrás crear _entities_. Por ejemplo, quizás quieras crear las entidades _Author \_y \_Book_. Para cada entidad, necesitarás:

* Una tabla en la base de datos
* Un _change set_ de Liquibase
* Una entidad JPA
* Un Repositorio JPA de Spring Data
* Un Controlador REST de Spring MVC, el cual tiene las operaciones básicas _CRUD_.
* Un _router_, un componente y un servicio de Angular.
* Una vista HTML
* Pruebas de integración, para validar que todo funciona como se espera
* Pruebas de rendimiento, para ver que todo funciona bien.

Si tienes varias entidades, probablemente desees tener relaciones entre ellas. Para esto necesitarás:

* Una llave foránea en la base de datos
* Código JavaScript y HTML específico  para administrar esta relación.

El sub-generador de "entidad" creará todos los archivos necesarios y proporcionará un CRUD _front-end _ para cada entidad \(consulta la [estructura del proyecto](http://www.jhipster.tech/using-angularjs/)\). El sub generador se puede invocar ejecutando `jhipster entity <entityName> --[options]`. La referencia para estas opciones se encuentra escribiendo `jhipster entity --help`.

A continuación se muestran las opciones soportadas:

* `--table-name <table_name>` - Por defecto JHipster generará un nombre de tabla basado en el nombre de tu entidad, si quieres tener un nombre de tabla diferente puedes hacerlo pasando esta opción.
* `--angular-suffix <suffix>` - Si quieres que todas tus rutas de Angular tengan un sufijo personalizado puedes pasarlo usando esta opción.
* `--regenerate` - Regenera una entidad existente sin hacer ninguna pregunta.
* `--skip-server` - Omite el código del lado del servidor y genera solo el códigodel lado del cliente
* `--skip-client` - Omite el código del lado del cliente y genera solo el código del lado del servidor
* `--db` - Especifica la base de datos cuando se omite la generación del lado del servidor, en otro caso no tiene efecto.

## JHipster UML y JDL Studio

Esta página describe cómo crear entidades con JHipster utilizando la interfaz estándar de línea de comandos. Si deseas crear muchas entidades, quizás prefieras usar una herramienta gráfica.

En ese caso, hay dos opciones:

* [JHipster UML](http://www.jhipster.tech/jhipster-uml/), el cual te permite usar un editor UML.
* [JDL Studio](http://www.jhipster.tech/jdl-studio/), nuestra herramienta _online_ para crear entidades y relaciones usando nuestro lenguaje de dominio específico [JDL](http://www.jhipster.tech/jdl/). 

Si usas el JDL Studio:

* Puedes generar entidades a partir de un archivo JDL usando el sub-generador `import-jdl`, ejecutando `jhipster import-jdl tu-archivo-jdl.jh`.
  * Si no quieres regenerar tus entidades, durante la importación del JDL, puedes usar la bandera`--json-only` para omitir la creación de entidades y crear solamente los archivos json en la carpeta .jhipster.
  * Por defecto `import-jdl` regenera solamente entidades que han cambiado, si deseas que todas tus entidades se regeneren entonces pasa la bandera `--force`. Por favor nota que esto sobreescribirá todos tus cambios locales a los archivos de las entidades.
* Si deseas usar JHipster UML en lugar del sub-generador `import-jdl`, necesitas instalarlo ejecutando `npm install -g jhipster-uml`, y enseguida ejecutar `jhipster-uml elNombreDeTuArchivo.jh`

## Campos de entidades

Para cada entidad, puedes agregar tantos campos como desees. Necesitarás introducir los nombres de los campos y sus tipos y JHipster generará por ti todo el código y la configuración requerida, desde la vista HTML hasta el \_changelog \_de Liquibase.

Tales campos no pueden contener palabras reservadas de las tecnologías que estás utilizando. Por ejemplo, si usas MySQL:

* No puedes usar las palabras reservadas de Java \(puesto que tu código no compilará\)
* No puedes usar palabras reservadas de MySQL \(ya que fallará la actualización del esquema de tu base de datos\).

## Tipos de campos

JHipster soporta varios tipos de campo. Este soporte depende de la base de datos de tu _backend_, por lo tanto utilizamos tipos de Java para describirlos: un String Java se almacena diferente en Oracle o Cassandra, y una de las fortalezas de JHipster es generar por ti el código correcto para acceder a la base de datos.

* `String`: Un String de Java. Su tamaño por defecto depende del _backend_ que se utilice \(si usas JPA, es por defecto de 255\), pero también puedes cambiarlo usando regla de validación \(poniendo un tamaño máximo `max`de 1024, por ejemplo\).
* `Integer`: Un entero de Java.
* `Long`: Un _long _ de Java.
* `Float`: Un flotante de Java.
* `Double`: Un doble de Java.
* `BigDecimal`: Un objecto _java.math.BigDecimal_, se usa cuando quieres cálculos matemáticos exactos \(con frecuencia se usa para operaciones financieras\).
* `LocalDate`: Un objecto _java.time.LocalDate_, se usa para manejar correctamente las fechas en Java.
* `Instant`: Un objeto _java.time.Instant_, se usa para representar un _timestamp_, un punto instantáneo en la línea de tiempo. 
* `ZonedDateTime`: Un objeto _java.time.ZonedDateTime_, se usa para representar una fecha y tiempo local en una determinada _timezone _\(normalmente para una reunión calendarizada\). Nota que las _time zones_ no son soportadas por las capas REST ni de persistencia por lo tanto probablemente deberías usar en su lugar `Instant`la mayoría de las veces. 
* `Boolean`: Un Booleano Java.
* `Enumeration`: Un objecto Enum de Java. Cuando se selecciona este tipo, el sub-generador te preguntará cuáles valores deseas en tu enumeración y creará una clase enum específica para almacenarlos. 
* `Blob`: Un objeto Blob, se usa para almacenar algunos datos binarios. Cuando se selecciona este tipo, el sub-generador te preguntará si deseas almacenar datos binarios genéricos, un objeto imagen o un CLOB \(text largo\). Las imágenes se manejan específicamente del lado de Angular, así que se puede mostrar al usuario final.

## Validación

Se puede configurar la validación para cada campo. Dependiendo del tipo de campo, estarán disponible diferentes opciones de validación.

La validación será generada automáticamente en:

* las vistas HTML, usando el mecanismo de validación de AngularJS
* los objetos  de dominio de Java, usando Bean Validation.

La validación de Beans \(Bean Validation\) se usará para validar automáticamente objetos de dominio cuando se usen en:

* Controladores Spring MVC REST \(usando la anotación  `@Valid`\)
* Hibernate/JPA \(las entidades se validarán automáticamente antes de guardarse\)

La información de validación también se usará para generar metadatos más precisos de las columnas de la base de datos.

* Los campos requeridos se marcarán como no nulos
* Los campos que tienen una longitud máxima tendrán el mismo tamaño de columna.

La validación tiene algunas limitaciones:

* No soportamos todas las opciones de validación de AngularJS y Bean Validation, sino que solo soportamos a aquellos que se encuentran en ambas APIs.
* Los patrones de expresiones regulares no funcionan igual en JavaScript que en Java, así que si configuras uno, podrías necesitar modificar uno de los patrones generados.
* JHipster genera pruebas unitarias que funcionan para entidades genéricas, sin conocer tus reglas de validación: es posible que las pruebas generadas no pasen las reglas de validación. En ese caso, necesitarás actualizar los valores de muestra para tus pruebas unitarias, para que pasen las reglas de validación.

## Relaciones de entidades

Las relaciones de entidades solo están disponible para bases de datos SQL. Es un tema bastante complejo, el cual tiene su propia página de documentación: [Manejando relaciones](http://www.jhipster.tech/managing-relationships/).

## Objetos de Transferencia de Datos \(DTOs\)

Por defecto las entidades de JHipster no utilizan DTOs, pero están disponible como una opción. Aquí la documentación: [Usando DTOs](http://www.jhipster.tech/using-dtos/).

## Filtrado

Opcionalmente, las entidades almacenadas en bases de datos SQL se pueden filtrar usando JPA. Aquí ladocumentación: [Filtrando tus entidades](http://www.jhipster.tech/entities-filtering/).

## Paginación

Por favor nota que la paginación no está disponible si creaste tu aplicación con Cassandra. Por supuesto que se añadirá esta característica en una futura versión.

La paginación utiliza el [Link header](http://tools.ietf.org/html/rfc5988), como en la [API de Github](https://developer.github.com/v3/#pagination). JHipster proporciona una implementación personalizada de esta especificación tanto en el servidor \(Spring MVC REST\) como en el cliente \(Angular JS\).

Cuando se genera la entidad, JHipster proporciona 4 opciones de paginación:

* Sin paginación \(en ese caso, no se pagina el _back-end_\).
* Un paginador simple, basado en el [Bootstrap pager](http://getbootstrap.com/components/#pagination-pager). 
* Un sistema completo de paginación, basado en el [componente de paginación de Bootstrap](http://getbootstrap.com/components/#pagination).
* Un sistema de scroll infinito, basado en la [directiva de scroll infinito](http://sroze.github.io/ngInfiniteScroll/). 

## Actualizando una entidad existente

La configuración de entidad se guarda en un archivo `.json` específico, en el directorio de JHipster. De tal modo que si ejecutas el sub-generador de nuevo, usando un nombre de entidad, puedes actualizar o regenerar la entidad.

Cuando ejecutas el sub-generador de entidades para una entidad existente, se te hará  una pregunta ¿Deseas actualizar la entidad? Esto reemplazará los archivos existentes para esta entidad, todo tu código personalizado será sobreescrito con las siguientes opciones:

* Sí, regenera la entidad - Esto simplemente regenera tu entidad. Tip: Se puede forzar pasando una bandera `--regenerate` cuando ejecutas el sub-generador.
* Si, agrega más campos y relaciones - Esto te presentará preguntas para añadir más campos y relaciones.
* Sí, quita campos y relaciones - Se te presentarán preguntas para eliminar de la entidad campos existentes.
* No, salir - Saldrá del sub-generador sin cambio alguno.

Quizá desees actualizar tu entidad por alguna de estas razones:

* Quieres añadir/eliminar campos y relaciones a una entidad existente
* Quieres resetear el código de tu entidad a su estado original
* Actualizaste JHipster y te gustaría tener tu entidad generada con las nuevas plantillas
* Modificaste el archivo de configuración `.json` \(el formato es muy parecido a las preguntas hechas por el generador, así que no es muy complicado\), por lo tanto tienes una nueva versión de la entidad.
* Has copiado/pegado el archivo `.json` y quieres una nueva entidad que sea muy similar a la entidad copiada.

Tip: para regenerar todas tus entidades de una sola vez, puedes usar los siguientes comandos \(quita la bandera `--force` para que se muestren las preguntas cuando los archivos hayan cambiado\).

* Linux & Mac: ``for f in `ls .jhipster`; do jhipster entity ${f%.*} --force ; done``
* Windows: `for %f in (.jhipster/*) do jhipster entity %~nf --force`

## Tutorial

Éste es un pequeño tutorial sobre cómo crear dos entidades \(un _Author_ y un _Book_\) que tienen una relación _uno-a-muchos_.

**Importante:** si quieres tener recarga en vivo \("live reload"\) de tu código JavaScript/TypeScript, necesitarás ejecutar gulp \(para JavaScript/AngularJS 1\) o yarn start \(para TypeScript/Angular 2+\). Para más información puedes visitar la página Utilizando JHipster en desarrollo.

### Generar la entidad "Author"

Como deseamos tener una relación entre Authors y Books \(un autor puede escribir varios libros\), necesitamos crear primero al Author. A nivel de base de datos, JHipster será capaz de agregar una clave foránea en la tabla Book, que enlace a la tabla Author.

jhipster entity author

Responde las siguientes preguntas acerca de los campos de esta entidad, el autor tiene:

* un "name" del tipo "String"
* un "birthDate" del tipo "LocalDate"

Después responde las preguntas relacionadas a las relaciones, el autor tiene:

* Una relación _uno-a-muchos_ con la entidad "book" \(la cual aún no existe\)

### Genera la entidad "Book"

jhipster entity book

Responde las preguntas siguientes relacionadas a los campos de esta entidad, el libro tiene:

* un "title" del tipo "String"
* una "description" del tipo "String"
* una "publicationDate", del tipo "LocalDate"
* un "price", del tipo "BigDecimal"

Después responde las preguntas relacionadas a las relaciones, el libro:

* Tiene una relación _muchos-a-uno_ con la entidad "author"
* Y esta relación utiliza el campo "nombre" \(de la entidad Author"\) para mostrarse

### Revisa el código generado

Ejecuta la suite de pruebas generadas, con mvn test, la cual probará la entidad Author y Book.

Lanza la aplicación \(por ejemplo con mvn\), inicia sesión y selecciona las entidades "Author" y "Book" en el menú "entidades".

Revisa las tablas de la base de datos, para ver que tus datos se insertaron correctamente.

### Mejora el código generado

Los archivos generados contienen todas las operaciones básicas CRUD y no necesitan modificarse si tus necesidades son simples.

Si deseas modificar el código generado o el esquema de base de datos, deberías seguir nuestra [guía de desarrollo](http://www.jhipster.tech/development/). 

Si deseas algunos comportamientos de negocio más complejos, tal vez necesitas añadir una clase @Service de Spring, usando el sub-generador de servicios.

### !Terminaste!

Tu página CRUD generada debe verse algo así:![](http://www.jhipster.tech/images/screenshot_5.png)



