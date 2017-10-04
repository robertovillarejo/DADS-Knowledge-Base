# Lineamiento - Maven

## Líneamiento para GrouId, ArtifactId y Version

---
### GroupId

El grupid deberá de identificar al proyecto de manera única con respecto a todos los proyectos de la dirección. Se deberá de seguir las reglas de nombrado de paquetes para java (ver [Java Convention](http://www.oracle.com/technetwork/java/codeconventions-135099.html) y [Packaging Convention](http://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html)), lo que implica contar con un nombre de dominio con el que se pueda controlar su evolución. Se podrán crear los subgrupos que se deseen. A continuación se muestran algunos ejemplos:

|        Sitio web      |            Groupid         |
|:---------------------:|:--------------------------:|
|www.infotec.mx         |mx.infotec.dads.arquitectura|
|maven.apache.org       |org.apache.maven            |
|apache.org             |org.apache.commons          |
|www.springframework.org|org.springframework         |
|oness.sf.net           |net.sf.oness                |
|github.com/yourusername|com.github.yourusername     |

Una buena estrategia para determinar la granularidad de un grupo es utilizar la estructura del proyecto, es decir, si el proyecto actual está compuesto de varios módulos, este debería agregar un nuevo identificador al groupid de su padre, por ejemplo:

#### Proyecto padre
`mx.infotec.dads.conacyt`
#### Sub-Módulos
`mx.infotec.dads.conacyt.finanzas` **(Sub-Modulo Finanzas)**
`mx.infotec.dads.conacyt.inventario` **(Sub-Modulo Inventario)**
`mx.infotec.dads.conacyt.seguridad` **(Sub-Modulo Seguridad)**

---
### ArtifactId

Es el nombre del `jar` sin la versión y debe estar siempre en minúsculas sin caracteres extraños. Ejemplo de nombrado: `maven`, `commons-math`

---
### Version

Se deberá de utilizar el estándar de [SEMANTIC 2.0](http://semver.org/)
