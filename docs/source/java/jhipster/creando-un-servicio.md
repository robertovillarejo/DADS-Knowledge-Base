# Creando un servicio

## Introducción

Comparado con el sub-generador "entity", éste es un sub-generador muy simple.

Su objetivo es generar un bean de Servicio de Spring, en el que se supone que se codifique la lógica de negocio de tu aplicación.

Para generar un bean de Servicio "Bar", simplemente escribe:

jhipster service Bar

Esto generará un simple "BarService": muy pocas líneas de código, pero generalmente surgen  varias preguntas. A continuación tratamos de responder las preguntas más comunes. 

## ¿Por qué las clases de Servicio no las genera el generador "entity"?

Aquí tenemos dos principios de arquitectura principales:

* No queremos promover la creación de Servicios sin uso: si todo lo que necesitas es un CRUD básico sobre tu base de datos, no necesitas un bean de Servicio. Es por eso que, por defecto, JHipster no los genera.
* Creemos que un bean de servicio es menos granular que un repositorio. Un bean de Servicio usa varios repositorios para proporcionar valor de negocio sobre ellos. Es por eso que los beans de Servicio no pueden generarse con las Entidades.

## ¿Deberíamos usar interfaces para nuestros Beans de Servicio?

Repuesta corta: **No**

Si deseas la respuesta larga, aquí está:

Uno de los principales intereses de usar Spring es AOP \(Aspect Oriented Programming: Programación Orientada a Aspectos\). Esta tecnología permite que Spring añada nuevos comportamientos sobre tus Beans:  así es cómo funcionan las transacciones o la seguridad, por ejemplo. 

A fin de añadir tales comportamientos, Spring necesita crear un proxy sobre tu clase y hay dos maneras de crear un proxy.

* Si tu clase usa una interfaz, Spring usará un mecanismo estándar proporcionado por Java para crear un proxy dinámico. 
* Si tu clase no usa un interfaz, Spring usará CGLIB para generar una nueva clase _al vuelo: _no es un mecanismo estándar de Java, pero funciona también como el mecanismo estándar.

Algunas personas dirán que las interfaces son mejores para escribir pruebas, pero creemos que no deberíamos modificar nuestro código para hacer pruebas y que todos esos nuevos _frameworks_ de maquetado \(como _EasyMock_\) te permiten crear buenas pruebas unitarias sin ninguna interfaz.

Así que, al final, encontramos que las interfaces para tus beans de Servicio son en su mayoría inútiles, y es por eso que no las recomendamos \(!aunque te dejamos la opción para generarlas!\).

## ¿Por qué debería usar transacciones para recuperar relaciones JPA _lazy_?

Por defecto JPA utiliza inicialización _lazy_ de las relaciones _uno-a-muchos _y _muchos-a-muchos_. Si quieres usar esta configuración por defecto, probablemente verás la temida LazyInitializationException: significa que trataste de usar una relación sin inicializar fuera de una transacción.

Como la clase de Servicio generada tiene por defecto la anotación @Transactional, todos sus métodos son transaccionales. Esto significa que puedes obtener todas la relación _lazy_ requerida dentro de esos métodos de negocio, sin ninguna LazyInitializationException.

Tip: usa @Transactional\(readOnly=true\) en un método si no estás modificando ningún dato. Éste es una buena optimización de rendimiento \(Hibernate no necesita hacer _flush_ de su caché de primer nivel, porque no estamos modificando nada\), así como una mejora de calidad son algunos drivers JDBC \(Oracle no permite enviar sentencias INSERT/UPDATE/DELETE\).

## ¿Podemos añadir seguridad a los Beans de Servicio?

!Sí! Solo añade la anotación @Secured de Spring Security en tu clase o en tus métodos y usa la clase Authorities Constants proporcionada para restringir el acceso a autoridades específicas de usuario.

## ¿Podemos monitorear los Beans de Servicio?

!Sí! Solo añade las anotaciones de Métricas @Timed en los métodos que quieres monitorear. 

