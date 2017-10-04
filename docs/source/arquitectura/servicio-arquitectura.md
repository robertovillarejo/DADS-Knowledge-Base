# Servicios
Cada ser

* Administración de Herramientas.
    [Infraestructura]
    * Administración
    * Soporte Técnico
    * Capacitación
    * Asesoría
* Administración de Activos de Arquitectura.

#### Procesos
* *Proceso de Arquitectura de Software.
* *Proceso de iCM
* *Proceso de Innovación (Fiware)

![Alt text](http://www.gravizo.com/img/1x1.png#
thiisthemark        
@startuml
object Object01
object Object02
object Object03
object Object04
object Object05
object Object06
object Object07
object Object08

Object01 <|-- Object02
Object03 *-- Object04
Object05 o-- "4" Object06
Object07 .. Object08 : some labels
@enduml
thiisthemark        
)


<img src='http://g.gravizo.com/g?
/**
*Structural Things
*@opt commentname
*@note Notes can
*be extended to
*span multiple lines
*/
class Structural{}

/**
*@opt all
*@note Class
*/
class Counter extends Structural {
        static public int counter;
        public int getCounter%28%29;
}

/**
*@opt shape activeclass
*@opt all
*@note Active Class
*/
class RunningCounter extends Counter{}
'>
