# El Rol de El Arquitecto de Software

## Introducción
### Propósito
Brindar una visión clara sobre las competencias necesarias para cubrir con el rol de arquitecto de software y sus funciones dentro de la DADS.

### Alcance

El presente trabajo tiene por alcance describir las competencias y responsabilidades deseadas en el Arquitecto de Software dentro del INFOTEC. La descripción detallada de las actividades y productos de trabajos quedan fuera del presente documento.

### Panorama

El presente documento está dividido en cuatro partes; Marco Histórico de la Arquitectura de Software, donde se lleva a cabo un resumen general sobre la historia de la disciplina; Competencias del Arquitecto de Software, donde se describen los dos tipos de competencias que deberá de cumplir un Arquitecto de Software; Responsabilidades del Arquitecto de Software, donde se describen las responsabilidades que deberá de llevar a cabo un Arquitecto de Software antes, durante y después del ciclo de vida de un proyecto; Actividades y Productos de Trabajo que ejecuta el Arquitecto de Software a lo largo de las fases del proyecto.

## Marco Histórico

### Introducción

En esta sección se revisan los principales acontecimientos históricos que dieron lugar a la Arquitectura de Software como tema de estudio, así como también se citan algunos precursores cuyo papel fue fundamental para la cimentación de conceptos y términos que hoy en día conforman a la disciplina.


### Marco Histrórico de la Arquitectura de Software

El estudio de la arquitectura de software se enfoca en gran medida en el estudio de la estructura de software, cuyo origen se monta a Edsger *Dijkstra* en el año 1968, donde habla acerca de la importancia de preocuparse sobre el establecimiento de una división y estructuración correcta de los sistemas de software antes de comenzar a programar, con el fin de producir un resultado correcto [PaulCLindaNorth96]. Después es David Parnas quien retoma la apreciación de *Dijkstra* y contribuye con sus ideas sobre módulos que oculten información, estructuras de software y familias de programas; entendiendo por familia de programas al conjunto de programas para los cuales es rentable o útil considerarlos como un grupo. Más tarde en la conferencia de la OTAN en 1969, P.I.Sharp, formula un conjunto de apreciaciones importantes, comentando sobre las ideas de *Dijkstra* y estableciendo la diferencia que existe entre la ingeniería de software y la arquitectura de software. Hasta la década de los 90´s, el término “arquitectura de software” se vio de distintas maneras, sobretodo se ve muy ligado al diseño. En esta década se habla de un nivel de atracción, sin embargo aún no estaba en su lugar los elementos que permitieran reclamar la necesidad de una disciplina y una profesión particular.
En el Software Engineering Insitute (SEI), se reconoce a “*Studying Software Architecture Through Design Spaces and Rules*”, de Thomas G. Lane, como el primer libro publicado por el SEI, sobre el tema de la Arquitecturass de Software escrito en 1990. En este libro, Lane establece una definición de Arquitectura de Software basada en el concepto expuesto por Mary Shaw, en el año de 1989, en su libro “*Larger Scale Systems Requiere Higher-Level Abstractions*” publicado por el IEEE *Computer Society*. En su libro Lane define la Arquitectura de Software como:

>“Arquitectura de Software es el estudio de la estructura a gran escala y el rendimiento de los sistemas de software. Aspectos importantes de la arquitectura de un sistema incluye la división de funciones entre los módulos del sistema, los medios de comunicación entre módulos, y la representación de la información compartida [Lane1990]”.

Con esos aportes, se ve que hay una búsqueda hacia un modelo con el que se pueda estructurar el software, dando lugar al diseño de un conjunto de modelos de dominios, basados en diseños genéricos, como son DSSA (*Domain-Specific Software Architectures*) elaborado por Mettala y Graham, en el año 1992.
Después, con el lanzamiento del libro “*An Introduction to Software Architecture*”, de Nart Sgaw t Davud Grkab [GarlanShaw1994], en el año 1994, donde se plantea, que debido al aumento del tamaño y cimplejidad de los productos de software, el problema principal ya no radicaba en la complejidad de los algoritmos y las estructuras de datos que se empleaban en los sitemas, sino que se dirige a la organización de los componentes que conforman el sistema, introduciendo de esta manera, la necesidad de la creación de la arquitectura de software, como disciplina científica.
El objetivo de estudio de la arquitectura de software es la determinación de un conjunto de paradigmas que establezcan una organización del sistema a alto nivel, la interrelación entre los distintos componentes que lo conforman y los principios que orientan su diseño y evolución.
El libro “*An introduction to Software Architecture*” se introduce por primera vez el término “*Architectural Styles*”, y se definen algunos de ellos, entre los que se encuentran; Tubería y filtros, Abstracción de datos y organización orientada a objetos, Repositorio, Arquitectura en Capas, Arquitectura basada en eventos, Table Driven Interpreters, entre otros.
Hasta ese momento el tema del rol de arquitecto de software dentro del proceso y desarrollo del software, no se expone a la luz, sin embargo, en el libro, titulado: “Pasado, presente y futuro”, se expone un grupo de áreas de interés para el estudio posterior de la disciplina, dentro de las que se destaca, “lograr un mejor entendimiento del rol del arquitecto en el ciclo de vida del proceso.”
En este mismo año, 1994, se realiza otro acontecimiento arquitectónico relevante. Mary Shaw y David Garlan, lanzan el concepto de Lenguajes de Descripción Arquitectónica (ADLs), en su libro “Characteristics of Higher Level Languages for Software Architecture”. En el estudio se señala las alternativas que hasta el momento se poseen para la definición de la arquitectura de software de un sistema. En primer lugar, a través de la modularización de las herramientas existentes de programación y de los módulos de conexión entre ellas y, en segundo lugar, describe sus diseños usando diagramas informales y frases idiomáticas [GarlanShaw011994]. A partir de estas reflexiones, definen un conjunto de regularidades y propiedades específicas, que constituyeron las bases de los ADLs. Después de los ADLs, la disciplina ha ido creciendo grandemente, destacándose un conjunto de acontecimientos como el libro “Coming Attractions in Software Architecture”, de Paul Clements, en el año 1996, donde define cinco temas fundamentales en torno de los cuales se agrupa la disciplina; diseño o selección de la arquitectura, representación de la arquitectura, evaluación y análisis, etc. Otro suceso relevante fue el lanzamiento de la tesis de Roy Fielding, en el año 2000, en la cual presenta el modelo REST, el cual establece definitivamente el tema de las tecnologías de Internet y los modelos orientados a servicios y recursos en el centro de las preocupaciones de la disciplina. En el mismo año se publica la versión definitiva de la recomendación IEEE Std 1471, que procura homogeneizar y ordenar la nomenclatura de descripción arquitectónica y homologar los estilos como un modelo fundamental de representación conceptual.

## Descripción del Rol
### Introducción

Durante las dos décadas pasadas, el rol del arquitecto de software ha cambiado a lo largo del proceso de desarrollo de software. A medida que las metodologías cambian es que han emergido un alcance y competencias complementarias dentro del rol, entre ellas las competencias suaves.

### Competencias Suaves
#### Lista de Competencias

Lista de las competencias suaves deseables en un Arquitecto de Software:

* Saber oír, aceptar sugerencias para recibir ayuda efectiva.
* Generar propósitos y alcanzarlos, generar compromisos y cumplirlos, buscar soluciones, generar aprendizaje a través del trabajo en equipo, generar una visión compartida y pensar de manera sistémica.
* Tener confianza en sí mismo y en los demás, tener preparación para enfrentar cualquier circunstancia, disposición para el cambio inmediato, habilidad para la solución de problemas, actitud de iniciativa, adecuada comunicación oral y escrita, facilidad para el manejo de idiomas foráneos.
* Habilidad para el buen manejo de las relaciones humanas y estar preparado para trabajar de manera continua en el tiempo.

La lista de competencias anteriores ayudan a mejorar las siguientes áreas:

1. Comunicación
2. Toma de Decisiones
3. Conocimiento de las Políticas de la Organización
4. Negociación.

En lo que resta de esta sección se describe a cada una de las áreas mencionadas con anterioridad.

#### Comunicación

La comunicación es una de las habilidades más importantes del arquitecto. El arquitecto tiene que tener habilidades para hablar, escribir y exponer. El arquitecto debe ser un buen oyente y observador, así como un buen orador. El arquitecto debe estimular el intercambio con sus compañeros, para que de esta manera se obtenga una rápida retroalimentación [Faber].

#### Toma de Decisiones

Un arquitecto que es incapaz de tomar decisiones en un entorno desconocido, donde no hay tiempo suficiente para explorar todas las alternativas posibles y donde existe una presión para entregar el trabajo deseado, es poco probable que tenga éxito. Los arquitectos exitosos reconocen la situación, en lugar de tratar de cambiarla. La incapacidad para tomar decisiones poco a poco socavara el proyecto y el equipo del proyecto perderá la confianza en el arquitecto. Por lo dicho anteriormente, es que el arquitecto deberá de ser uno de los creadores y poseedores de las grandes decisiones, centrándose en la coordinación externa [Abrahamsson] y ser el responsable de decidir sobre el tiempo y cantidad de la refactorización que se realizará [Babar2009]. Además, el arquitecto deberá tomar decisiones sobre métodos para descomponer el trabajo en formas que sean manejables en cada iteración, entrega o compromisos [Madison 00]

#### Conocimiento de las Políticas de la organización

El arquitecto de éxito no es sólo técnico. También son políticamente astutos y conscientes de que el poder reside en una organización. Este conocimiento se utiliza para asegurar que las personas adecuadas se comunican con él.

#### Negociación

Dadas las muchas dimensiones del diseño de la arquitectura, el arquitecto interactúa con muchos de los stakeholders del proyecto. Algunas de estas interacciones requieren de habilidades para la negociación. Por ejemplo, el arquitecto tiene que minimizar el riesgo tan pronto como sea posible en el proyecto, una manera de remover un riesgo es volver a redefinir los requerimientos, de tal manera que los stakeholders y el arquitecto lleguen a un acuerdo mutuo. Esta situación requiere que el arquitecto sea un negociador efectivo. El arquitecto de software, como negociador, deberá saber apoyar al líder del equipo en la priorización de los requerimientos del usuario durante la creación del plan de trabajo [Madison 00]. Durante la creación del plan de trabajo, el arquitecto deberá expresarles al cliente y al líder la importancia de ciertas tareas que son relevantes para estabilizar la arquitectura del sistema, sobre algunas prioridades del negocio u organización, y que esta prioridad más adelante permitirá reducir tiempos de refactorización originando que las entregas puedan ser rápidas y oportunas.

### Competencias Duras
#### Lista de Competencias Duras (Técnicas)

Lista de las competencias duras deseables en un Arquitecto de Software:

* Administración de los requerimientos no funcionales.
* Selección de la  tecnología.
* Mejora continua de la arquitectura.
* Facilitador.
*  y formador.
*  de la Calidad.  

Otras competencias:  

* Examinar los factores de impacto no técnico en la arquitectura del software.
* Identificar los aspectos humanos que se involucran en la arquitectura de software; incluyendo los compromisos de arquitectura existentes.
* Averiguar lo que la arquitectura deberá de hacer e identificar el tipo de soporte necesario para resolver oportunamente.


#### Dominio del Negocio

El arquitecto de software deberá de tener una comprensión sobre el dominio del negocio bajo el cual se deberá de crear la arquitectura. Al tener el conocimiento sobre el dominio del negocio, el arquitecto de software debe ser capaz de dibujar requerimientos del negocio en la forma de storyboard, u otro, explicar las restricciones técnicas para el negocio y reafirmar las necesidades del negocio en términos técnicos para el equipo [Madison 00]. Además, como proveedor de servicios, el arquitecto de software debe darse cuenta que la arquitectura no es final ni estática, sino un objeto de cambio como la comprensión del sistema que se incrementa durante su desarrollo. Para mantenerse actualizado, el arquitecto debe actualizar continuamente sus conocimientos sobre el dominio de aplicación [Faber]; esto implica una disciplina de auto capacitación.

#### Conocimientos Tecnológicos

Ciertos aspectos de la arquitectura claramente requieren un conocimiento de la tecnología, por lo tanto, un arquitecto debe mantener un cierto nivel de habilidades tecnológicas. Debido a que la tecnología cambia rápidamente, es necesario que el arquitecto se mantenga al tanto de esos cambios.

#### Conocimientos de Diseño

El arquitecto debe poseer fuertes habilidades de diseño, ya que la arquitectura contempla las principales decisiones de diseño. Tales decisiones representarán las principales decisiones sobre el diseño de toda la estructura del sistema, entre estas decisiones de diseño encontramos: la elección de un patrón particular, una táctica arquitectónica, un estilo arquitectónico, protocolos de comunicación, etc. Adicionalmente, el arquitecto deberá asegurar que la refactorización no tenga efectos negativos en la arquitectura del sistema [Babar2009].

#### Conocimientos de Programación

Los desarrolladores de un proyecto representan uno de los grupos más importantes con los que el arquitecto deberá interactuar; después de todo, ellos producen el software final. La comunicación entre el arquitecto y los desarrolladores puede ser efectiva sólo si el arquitecto es considerado importante dentro del trabajo de los desarrolladores. Por lo tanto, el arquitecto debe tener un cierto nivel de habilidades para programar. El arquitecto, al ser el responsable de los atributos de calidad de un sistema, debe actuar como maestro programador, quien se asegurará de que la arquitectura del sistema sea entendida por los desarrolladores y la calidad de todo el código del sistema sea preservada [Faber].

### Responsabilidades
#### Introducción

En esta sección se presentan las principales responsabilidades del arquitecto de Software, sus roles en la organización y su involucramiento en cada una de las áreas de la organización.

#### Responsabilidades del Arquitecto por Área

En la siguiente tabla se muestran los diferentes roles que puede tomar un arquitecto y las áreas que influencia en mayor o menor medida [Sofia2015]:

#### Negocio y Ventas

Las actividades de negocio y ventas recaen en la responsabilidad del equipo de ventas y se llevan a cabo antes de que el proyecto comience. Las salidas de las actividades de negocio y ventas son utilizadas por los administradores del proyecto cuando desarrollan el documento de requerimientos del sistema. El rol del arquitecto dentro de estas actividades es dar soporte y asesoramiento al equipo de ventas para definir las  características que deberá de presentar el producto. El arquitecto no produce ningún documento, más bien, sus actividades son las de un asesor técnico. Los compromisos del arquitecto durante la fase de negocio y ventas incluyen actividades tales como: transformar la estrategia de negocio en la visión técnica y estratégica; comprender y evaluar los procesos de negocio, ayudando a la organización a cumplir con sus metas de negocio;  comprender las necesidades del cliente de acuerdo a sus tendencias en el mercado. En la práctica, durante esta fase el arquitecto aprende de su cliente y del mercado en el cual se encuentra influenciado, para que de esta manera se brinde paso a la comunicación y cooperación con el equipo de negocio. El arquitecto necesita comprender las presiones del mercado bajo las cuales se encuentra su cliente; entender que tipos de cambios son importantes para el cliente y su organización; el arquitecto debe participar activamente en la conceptualización de como el negocio responde a estas presiones.

#### Administración de Proyectos

Las actividades en la que deberá de participar el arquitecto de software durante las actividades de planeación son las referentes a la estimación, descomposición del trabajo, asesoría y revisor del trabajo que será llevado a cabo por los equipos. El arquitecto no es el responsable de generar los productos concernientes a esta disciplina, pero si será el responsable de brindar soporte para poder obtenerlos.

#### Levantamiento y Análisis de requerimientos

Durante el levantamiento de requerimientos, el arquitecto de software deberá de asegurarse que las necesidades funcionales y no funcionales de los stakehoders se encuentran capturadas de manera adecuada. Además, el arquitecto deberá de monitorear los requerimientos capturados y asegurarse que estos puedan derivarse en una solución de alto nivel [Sofia2015].

#### Revisiones de la Arquitectura

La revisión de la arquitectura de software es una fase crítica dentro del proceso del desarrollo de software. Durante esta fase, el arquitecto deberá de hacerse consiente de las decisiones de diseño que se han tomado y, en su caso, corregir los elementos de diseño que no se han considerado. Este proceso será de índole iterativo debido a su naturaleza evolutiva [Sofia2015].

#### Programación y Pruebas

La programación y las pruebas deberán de ser de interés relevante para el arquitecto. Si bien se sabe, el arquitecto no es el responsable pero deberá de cuidar que sus políticas, lineamientos y reglas de arquitectura se cumplan a lo largo del desarrollo y pruebas del sistema bajo construcción. En cuanto al tema de ser mentor durante el proceso de desarrollo de software, podemos destacar las siguientes actividades: una vez que la arquitectura inicial está construida, el arquitecto deberá proveer guía y supervisión que permita asegurar que el diseño y patrones elegidos están siendo seguidos de manera adecuada. Adicionalmente, el arquitecto deberá ayudar al equipo de desarrollo en la solución de los problemas complejos.

#### Documentación, mantenimiento y reutilización

La arquitectura de un sistema es uno de los productos de trabajo más complejos de documentar. Este documento ayuda a mantener consistencia cuando se toman decisiones sobre el diseño de la arquitectura y facilita la comunicación con otros stakeholders del sistema [AgileDocumentation]. Uno de sus principales propósitos es brindar información sobre la estructura del sistema, como funciona y las principales decisiones técnicas que se tomaron para llegar a su concepción [Sofia2015]. A partir de este documento es que se puede llegar a la reutilización de componentes, misma que el arquitecto será responsable. Es responsabilidad del arquitecto de software la creación inicial de este documento y, en lo subsecuente, será el responsable de solicitar la información requerida para evolucionarlo. El documento de arquitectura, en general, es creado por los siguientes roles:  
* Arquitecto(s) de Software (Líder)
* Administrador de Programa (participante)
* Líder del Equipo (Participante)
* Analista(s) de Negocio (participante)
* Analista(s) de Sistemas (participante)
* Ingeniero(s) de Pruebas (Participante)
* Ingeniero(s) de Software (Participante)
