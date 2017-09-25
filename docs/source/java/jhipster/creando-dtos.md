# Usando DTO's \[BETA\]

**!ADVERTENCIA! **Ésta es una característica nueva, de calidad **BETA**. !Úsala bajo tu propio riesgo! !La retroalimentación es bienvenida!

## Introducción

Por defecto, JHipster utiliza sus objetos de dominio \(normalmente entidades JPA\) directamente en sus _endpoint _REST. Esto tiene un montón de beneficios, el principal es que hace que el código sea fácil de leer, entender y extender.

Sin embargo, para casos de uso complejos, es posible que desees usar DTO's \(Data Transfer Object: Objectos de Transferencia de Datos\) los cuales se expongan en los _endpoint_ REST. Esos objetos añaden una capa adicional a los objetos de dominio y son ajustados específicamente para la capa REST: su principal beneficio es que pueden agregar varios objetos de dominio.

## ¿Cómo funcionan los DTO's en JHipster?

Cuando se genera una entidad JHipster, tienes la opción de generar también un DTO para esa entidad. Si seleccionas dicha opción:

* Se generará un DTO y se mapeará a la entidad subyacente.
* Se agregarán relaciones _muchos-a-uno_, sólo usando el _ID_ y el campo para mostrarlo con AngularJS. Por ejemplo, una relación _muchos-a-uno_ con la entidad User añadirá un campo _`userId`_ y un campo _`userLogin`_ al DTO.
* Se ignorarán las relaciones _uno-a-muchos_ y _muchos-a-muchos_ del lado no propietario: esto coincide con la manera en que funcionan las entidades \(éstas tienen una anotación `@JsonIgnore` en esos campos\). 
* Para una relación _muchos-a-muchos_ del lado del propietario: se usarán DTOs de la otra entidad y se usarán en un `Set`. Por tanto, esto funciona solo si la otra entidad también usa DTOs. 

## Usando _MapStruct_ para mapear DTOs y entidades

Dado que los DTOs se parecen mucho a las entidades, un requerimiento frecuente es tener una solución para mapear automáticamente una con otra. 

La solución seleccionada en JHipster es usar [_MapStruct_](http://mapstruct.org/)_. _Es un procesador de anotaciones, conectado al compilador Java, que genera automáticamete el mapeo requerido. 

Lo hemos encontrado muy limpio y eficiente y nos gustó que no usa reflección \(lo cual es malo para el rendimiento cuando se usa tanto como en un _mapper_\). 

## Configurando tu IDE para _MapStruct_

_MapStruct_ es un procesador de anotaciones y como tal debería prepararse automáticamente para la ejecución cuando el IDE compila el proyecto.

Si estás usando Maven, necesitas activar el perfil de Maven `IDE` en tu IDE. Los usuarios de Gradle no necesitan aplicar nada específico del IDE.

Las instrucciones para activar el perfil se incluyen en Configurando tu IDE. 

## Uso avanzado de _MapStruct_

Los _mappers_ de _MapStruct_ se configuran como Beans de Spring y soportan inyección de dependencias. Un buen tip es que puedes inyectar un Repository en un mapper, así puedes obtener una entidad JPA desde el mapper, usando su ID.

Aquí se muestra un ejemplo de código, obteniendo una entidad User:

```java
@Mapper
public abstract class CarMapper {

    @Inject
    private UserRepository userRepository;

    @Mapping(source = "user.id", target = "userId")
    @Mapping(source = "user.login", target = "userLogin")
    public abstract CarDTO carToCarDTO(Car car);

    @Mapping(source = "userId", target = "user")
    public abstract Car carDTOToCar(CarDTO carDTO);

    public User userFromId(Long id) {
        if (id == null) {
            return null;
        }
        return userRepository.findOne(id);
    }
}
```

## Trabajos futuros

En e futuro, si nos damos cuenta de que _MapStruct_ no es suficiente para todos, añadiremos otro mapper DTO. La solución más sencilla sería generar ese mapeo automáticamente con JHipster y no usar una biblioteca de terceros. 



