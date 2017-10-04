# JHipster Domain Language \(JDL\)

JDL es una lenguaje de dominio específico de JHipster donde hemos añadido la posibilidad de describir todas tus entidades y sus relaciones en un único archivo \(o más de uno\) con una sintaxis simple y amigable con el usuario.

Puedes usar nuestro IDE _online_ [JDL-Studio](http://www.jhipster.tech/jdl-studio/) para crear JDL y visualizarlo como UML. También puedes crear y exportar la URL de tu modelo JDL.

Una vez que tengas un proyecto generado \(ya sea uno existente o generado con la línea de comando `jhipster`\), puedes generar las entidades a partir de un archivo JDL usando el sub-generador import-jdl, ejecutando jhipster import-jdl tu-archivo-jdl.jh \(asegúrate de ejecutar este comando dentro de la carpeta de tu proyecto JHipster\). También puedes generar entidades y exportarlas como un archivo JDL, usando JHipster UML, ejecutando jhipster-uml yu-archivo-xmi.xmi --to-jdl desde la carpeta raíz de tu proyecto JHipster. Para aprender más sobre JHipster UML, e instalarlo, mira la documentación de [JHipster UML](http://www.jhipster.tech/jhipster-uml/).

Esto se puede usar como un reemplazo al uso del sub-generador de entidades. La idea es que es mucho más fácil administrar relaciones usando una herramienta visual que con las clásicas preguntas y respuestas de Yeoman.

El proyecto JDL está [disponible en Github](https://github.com/jhipster/jhipster-core/), es un proyecto _Open Source_ como JHipster \(Licencia Apache 2.0\). También se puede usar como una biblioteca de Node para _parsear_ JDL.

Si te gustas el Lenguage de Dominio de JHipster, !no olvides darnos una estrella en [Github](https://github.com/jhipster/jhipster-core/)! Si te gustas el JDL Studio !no olvides darnos una estrella en [Github](https://github.com/jhipster/jdl-studio/)!

Aquí está la documentación completa de JDL:

1. Muestra JDL
2. Cómo usarlo
3. El lenguaje
   1. Declaración de Entidades
   2. Declaración de Relaciones
   3. Numeraciones
   4. Blobs
   5. Declaración de opciones
   6. Opciones relacionadas con microservicios
4. Comentando
5. Todas las relaciones
6. Constantes
7. Anexos
8. _Issues_ y _bugs_

## Muestra JDL

Se tradujo a JDL la aplicación de muestra "Human Resources" de Oracle y se encuentra disponible [aquí](https://github.com/jhipster/jhipster-core/blob/master/lib/dsl/example.jh). La misma aplicación se carga por defecto en [JDL-Studio](http://www.jhipster.tech/jdl-studio/).

### Cómo usarlo

Si quieres usar JHipster UML en lugar del sub-generador import-jdl necesitas instalarlo ejecutando npm install -g jhipster-uml.

Luego puedes usar archivos JDL para generar entidades:

* Simplemente crea un archivo con la extensión '.jh' o '.jdl'
* Declara tus entidades y relaciones o créalas y descárgalas con [JDL-Studio.](http://www.jhipster.tech/jdl-studio/)
* En la carpeta raíz de tu aplicación JHipster, ejecuta jhipster import-jdl mi_archivo.jdl o jhipster-uml mi\_archivo.jdl_

y Voilà, !terminaste!

Si trabajas en equipo, tal vez te gustaría tener múltiples archivos en lugar de uno. Añadimos esta opción para que no tengas que concatenar manualmente todos los archivos en uno, solo tienes que ejecutar jhipster import-jdl mi_archivo1.jh mi\_archivo.jh o jhipster-uml mi\_archivo.jh mi\_archivo.jh._

Si no quieres regenerar tus entidades, mientras importas un JDL, puedes usar la bandera --json-only para omitir la creación de entidades y crear solamente los archivos jhipster en la carpeta .jhipster.

```
jhipster import-jdl ./mi-archivo-jdl.jdl --json-only
```

Por defecto import-jdl regenera solo entidades que hayan cambiado, si quieres que todas tus entidades se regeneren entonces pasa la bandera --force. Por favor nota que esto sobreescribirá todos tus cambios locales en los archivos de las entidades.

```
jhipster import-jdl ./mi-archivo-jdl.jdl --force
```

Si quieres usarlo en tu proyecto, puedes instalarlo localmente con npm install jhipster-core --save y guardarlo en tu archivo package.json.

## El lenguaje

Tratamos de mantener la sintaxis amigable como podemos para los desarrolladores. Puedes hacer tres cosas con él:

* Declarar entidades con sus atributos
* Declarar las relaciones entre ellas
* Y declarar algunas opciones específicas de JHipster

### Declaración de Entidades

La declaración de entidades se hace así:

```
entity <entity-name> {
    <field name> <type> [<validation>]
}
```

* &lt;entity name&gt; es el nombre de la entidad
* &lt;field name&gt; el nombre de un campo de la entidad
* &lt;type&gt; el tipo de campo soportado por JHipster
* y opcionalmente &lt;validation&gt; para hacer validaciones del campo.

Los posibles tipos y validaciones se describen [aquí](http://www.jhipster.tech/jdl/#annexes), si la validación requiere un valor, simplemente añade \(&lt;value&gt;\) a la derecha del nombre de la validación.

Aquí hay un ejemplo de código JDL:

```
entity A
entity B
entity C {}
entity D {
  name String required,
  address String required maxlength(100),
  age Integer required min(18)
}
```

Debido a que el JDL se hizo simple de usar y leer, si tu entidad está vacía \(no tiene campos\), puedes simplemente declarar una entidad con entity A o entity A {}.

Nota que JHipster añade por defecto un campo id así que no tienes que preocuparte por él.

### Declaración de relaciones

La declaración de relaciones se hace de esta manera:

```
relationship (OneToMany | ManyToOne | OneToOne | ManyToMany) {
  <from entity>[{<relationship name>[(<display field>)]}] to <to entity>[{<relationship name>[(<display field>)]}]
}
```

* \(OneToMany \| ManyToOne \| OneToOne \| ManyToMany\) es el tipo de tu relación.
* &lt;from entity&gt; es el nombre de la entidad propietario de la relación: la fuente,
* &lt;to entity&gt; es el nombre de la entidad a donde se dirige la relación: el destino,
* &lt;relationship name&gt; es el nombre del campo que tiene el otro extremo como tipo,
* &lt;display field&gt; es el nombre del campo que debería mostrarse en los _select boxes_ \(por defecto es el id\),
* required si el campo inyectado se requiere o no.

Aquí un ejemplo simple:

Un Book tiene un Author el cual es requerido, un Author tiene varios Books.

```
entity Book
entity Author

relationship OneToMany {
  Author{book} to Book{writer(name) required}
}
```

Por supuesto, en casos reales, tendrías un montón de relaciones y escribir siempre las mismas tres líneas podría ser tedioso. Es por eso que puedes declarar algo como esto:

```
entity A
entity B
entity C
entity D

relationship OneToOne {
  A{b} to B{a},
  B{c} to C
}
relationship ManyToMany {
  A{d} to D{a},
  C{d} to D{c}
}
```

La unión siempre se hace usando el campo id el cual también es el campo por defecto que se muestra cuando se edita una relación en el \_front-end. \_Si se debería mostrar otro campo en lugar de ese, puedes especificarlo así:

```
entity A {
  name String required
}
entity B


relationship OneToOne {
  A{b} to B{a(name)}
}
```

Esto hace que JHipster genere un recurso REST que retorne ambos el id y el nombre de la entidad ligada al \_front-end, \_así que se muestra al usuario el nombre en lugar del id.

### Enumeraciones \(Enum\)

Para hacer enumeraciones con JDL solo haz lo siguiente:

* Declara un Enum donde quieras en el archivo:

```
enum Language {
    FRENCH, ENGLISH, SPANISH
  }
```

* En una entidad, añade campos con el Enum como tipo:

```
entity Book {
    title String required,
    description String,
    language Language
  }
```

### Blob \(byte\[\]\)

JHipster proporciona una magnífica opción que uno puede elegir entre un tipo de imagen o cualquier tipo binario. JDL te permite hacer lo mismo, solo  crea un tipo personalizado \(mira DataType\) con el editor, nómbralo de acuerdo a estas convenciones:

* AnyBlob o solo Blob para crear un campo de cualquier tipo binario
* ImageBlob para crear un campo destinado a ser una imagen.
* TextBlob para crear un campo para un CLOB \(texto largo\)

Y puedes crear tantos DataTypes como quieras.

### Declaración de opción

En JHipster, puedes especificar opciones para tus entidades tales como paginación o DTO. Puedes hacer lo mismo con el JDL:

```
entity A {
  name String required
}

entity B {}

entity C {}

dto A, B with mapstruct

paginate A with infinite-scroll
paginate B with pagination
paginate C with pager  // pager is only available in AngularJS

service A with serviceClass
service C with serviceImpl
```

Se añadieron a la gramática las palabras clave dto, paginate, service y with para soportar esos cambios. Si se especifica una opción errónea, JDL te lo informará con un bonito mensaje rojo y lo ignorará para no dañar los archivos JSON de JHipster.

#### Opción Service

Si no se especifican servicios se creará una clase Resource la cual utilizará directamente la interfaz del Repositorio. Ésta es la opción más simple y por defecto, véase A. Un servicio con una serviceClass \(véase B\) creará un Resource llame la clase servicio la cual a su vez llamará a la interfaz de Repositorio. Un servicio con serviceImpl \(véase C\) creará una interfaz de servicio que será usada por la clase Resource. La interfaz es implementada por una clase impl la cual llamará a la interfaz de Repositorio.

No uses servicios si no estás seguro que es la opción más simple y lo mejor para tu CRUD. Usa servicios con una Clase si tendrás mucha lógica de negocio que usará múltiples repositorios, es el escenario ideal para una clase servicio. JHipster no es fan de interfaces innecesarias pero si las prefieres usa servicios con implementaciones.

```
entity A {}
entity B {}
entity C {}

// no service for A
service B with serviceClass
service C with serviceImpl
```

JDL también soporta configuración masiva, es posible hacer:

```
entity A
entity B
...
entity Z

dto * with mapstruct
service all with serviceImpl
paginate C with pagination
```

Nota que \* y all son equivalentes. La última versión introduce exclusiones \(que es una poderosa opción cuando estableces opciones para cada entidad\):

```
entity A
entity B
...
entity Z

dto * with mapstruct except A
service all with serviceImpl except A, B, C
paginate C with pagination
```

Con JHipster, también puedes indicar si no quieres código cliente, o código servidor. Incluso si deseas añadir un sufijo a los archivos de Angular, puedes hacer eso en JHipster. En tu archivo JDL, simplemente añade estas líneas para hacer lo mismo.

```
entity A
entity B
entity C

skipClient for A
skipServer for B
angularSuffix * with mySuperEntities
```

Por último, también se puede especificar los nombres de las tablas \(por defecto se usan los nombres de las entidades\):

```
entity A // El nombre de la tabla es A
entity B (the_best_entity) // El nombre de la tabla es the_best_entity
```

#### Opciones de microservicios

En JHipster v3, puedes crear microservicios. Puedes especificar algunas opciones para generar tus entidades en el JDL: el nombre del microservicio y el motor de búsqueda.

Aquí se muestra cómo puedes especificar el nombre de tus microservicios \(el nombre de la aplicación JHipster\):

```
entity A
entity B
entity C

microservice * with mysuperjhipsterapp except C
microservice C with myotherjhipsterapp
search * with elasticsearch except C
```

La primera opción se usa para decirle a JHipster que quieres tu microservicio lidie con tus entidades, mientras que la segunda opción especifica cómo y si deseas que tus entidades sean _buscables._

### Comentando y Javadoc

Es posible añadir Javadoc y comentarios a los archivos JDL.

Tal como en Java, este ejemplo demuestra cómo añadir comentarios Javadoc:

```
/**
 * Comentarios de clase.
 * @author El equipo de JHipster.
 */
entity MyEntity { // otra forma de comentario
  /** Un atributo requerido */
  myField String required,
  mySecondField String // otra forma de comentario
}

/**
 * Segunda entidad.
 */
entity MySecondEntity {}

relationship OneToMany {
  /** También se puede comentar así! */
  MyEntity{mySecondEntity}
  to
  /**
   * Y así también!
   */
  MySecondEntity{myEntity}
}
```

Estos comentarios posteriormente son añadidos como Javadoc por JHipster.

JDL posee su propio tupo de comentario:

```
// un comentario ignorado
/** un comentario no ignorado
```

Por lo tanto, cualquier línea que inicie con // es considerada como un comentario interno de JDL y no se contará como Javadoc. Por favor nota que las directivas de JDL Studio que inician con \# se ignoran durante el _parseo_.

Otra forma de comentarios son los siguientes:

```
entity A {
  name String /** Mi super campo */
  count Integer /** Mi otro super campo */
}
```

Aquí el campo _name_ de A se comentará con Mi super campo, B con Mi otro super campo. Sí, las comas no son obligatorias pero es mejor tenerlas para no tener errores en el código. Si deseas mezclar comas y los comentarios siguientes, ten cuidado!

```
entity A {
  name String, /** My comment */
  count Integer
}
```

El campo name no está comentado, porque el comentario lo tiene el campo count.

### Todas las relaciones

Explicación sobre cómo crear relaciones con JDL.

#### Uno-a-Uno

Una relación bidireccional donde Car tiene un Driver y Driver tiene un Car.

```
entity Driver {}
entity Car {}
relationship OneToOne {
  Car{driver} to Driver{car}
}
```

Un ejemplo unidireccional donde un Citizen tiene un Passport pero el Passport no tiene acceso a su únido dueño.

```
entity Citizen {}
entity Passport {}
relationship OneToOne {
  Citizen{passport} to Passport
}
```

#### Uno-a-Muchos

Una relación bidireccional donde el Owner tiene ningún, uno o más objectos Car y, Car conoce quién es su propietario.

```
entity Owner {}
entity Car {}
relationship OneToMany {
  Owner{car} to Car{owner}
}
```

Las versiones unidireccionales de esta relación no son soportadas por JHipster, pero sería algo así:

```
entity Owner {}
entity Car {}
relationship OneToMany {
  Owner{car} to Car
}

```

#### Muchos-a-Uno

La versión recíproca de las relaciones Uno-a-Muchos es la misma que la anterior. La versión unidireccional donde el Car conoce quiénes son sus propietarios:

```
entity Owner {}
entity Car {}
relationship ManyToOne {
  Car{owner} to Owner
}
```

#### Muchos-a-Muchos

Por último, en este ejemplo tenemos que Car conoce quiénes son sus conductores y, el objeto Driver puede acceder a sus carros.

```
entity Driver {}
entity Car {}
relationship ManyToMany {
  Car{driver} to Driver{car}
}
```

Por favor nota que el lado propietario de la relación tiene que estar del lado izquierdo

### Constantes

Así como el JHipster Core v1.2.7, JDL soporta constantes numéricas. Aquí un ejemplo:

```
DEFAULT_MIN_LENGTH = 1
DEFAULT_MAX_LENGTH = 42
DEFAULT_MIN_BYTES = 20
DEFAULT_MAX_BYTES = 40
DEFAULT_MIN = 0
DEFAULT_MAX = 41

entity A {
  name String minlength(DEFAULT_MIN_LENGTH) maxlength(DEFAULT_MAX_LENGTH)
  content TextBlob minbytes(DEFAULT_MIN_BYTES) maxbytes(DEFAULT_MAX_BYTES)
  count Integer min(DEFAULT_MIN) max(DEFAULT_MAX)
}
```

### Anexos

Aquí los tipos soportados por JDL:  

| SQL | MongoDB | Cassandra | Validaciones |
| :--- | :--- | :--- | :--- |
| String | String | String | required, minlength, maxlength, pattern |
| Integer | Integer | Integer | required, min, max |
| Long | Long | Long | required, min, max |
| BigDecimal | BigDecimal | BigDecimal | required, min, max |
| Float | Float | Float | required, min, max |
| Double | Double | Double | required, min, max |
| Enum | Enum |  | required |
| Boolean | Boolean | Boolean | required |
| LocalDate | LocalDate |  | required |
|  |  | Date | required |
| ZonedDateTime | ZonedDateTime |  | required |
|  |  | UUID | required |
| Blob | Blob |  | required, minbytes, maxbytes |
| AnyBlob | AnyBlob |  | required, minbytes, maxbytes |
| ImageBlob | ImageBlob |  | required, minbytes, maxbytes |
| TextBlob | TextBlob |  | required, minbytes, maxbytes |
| Instant | Instant | Instant | required |


### Issues y bugs

JDL está disponible en Github y sigue las mismas líneas de contribución que JHipster.

Por favor usa nuestro proyecto para enviar issues y Pull Requests relacionadas a la propia librería.

* Seguidor de problemas \(issues\) JDL
* Pull Requests de JDL

Cuando envíes algo, debes ser lo más preciso posible:

* **Cada issue publicado debe tener solamente un problema **\(o una petición/pregunta\)
* Los Pull Requests son bienvenidos pero los _commits _deben ser 'atómicos' para ser realmente comprensibles.
