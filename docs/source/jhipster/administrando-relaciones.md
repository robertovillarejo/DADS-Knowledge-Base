# Administrando relaciones

Cuando se usa JPA, el sub-generador de entidades puede crear relaciones entre las entidades.

## Presentación

Las relaciones solo funcionan cuando se usa JPA. Si eliges usar Cassandra o MongoDB, éstas no estarán disponibles.

Una relación funciona entre dos entidades y JHipster genera el código para:

* Administrar esta relación con JPA en las entidades generadas
* Crear el _changelog_ correcto de Liquibase, a fin de que la relación exista en la base de datos
* Generar el _front-end_ en AngularJS así puedes administrar esta relación de manera gráfica en la interfaz de usuario

## JHipster UML y JDL Studio

Esta página escribe cómo crear relaciones con JHipster usando la interfaz de línea de comandos estándar. Si deseas crear muchas entidades y relaciones, quizá prefieras usar una herramienta gráfica.

En ese caso, hay dos opciones disponibles:

* JHipster UML, el cual te permite usar un editor UML.
* JDL Studio, nuestro herramienta _online_ para crear entidades y relaciones usando nuestro lenguaje específico de dominio.

Puedes generar entidades con relaciones a partir de un archivo JDL usando el sub-generador `import-jdl`, ejecutando `jhipster import-jdl tu-archivo.jh`

## Relaciones disponibles

Como usamos JPA, se encuentran disponibles las relaciones usuales como _uno-a-muchos_, _muchos-a-uno, muchos-a-muchos_ y _uno-a-uno:_

1. Una relación bidireccional _uno-a-muchos_
2. Una relación unidireccional _muchos-a-uno_
3. Una relación unidireccional _uno-a-muchos_
4. Dos relaciones _uno-a-muchos_ en las mismas dos entidades
5. Una relación _muchos-a-muchos_
6. Una relación _uno-a-uno_
7. Una relación unidireccional _uno-a-uno_

Tip: la entidad `User`

Por favor nota que la entidad `User`, la cual maneja JHipster, es específica. Puedes hacer:

* relaciones `muchos-a-uno` a esta entidad \(un `Car `puede tener una relación _muchos-a-uno_ a un `User`_\). _Esto generará un _query_ específico en el repositorio de tu nueva entidad, así que puedes filtrar tu entidad por el usuario autenticado quien hace la consulta, lo cual es un requerimiento común. En la interfaz gráfica del cliente \(generada en AngularJS\) tendrás un _dropdown_ en `Car `para seleccionar un `User`.
* relaciones `muchos-a-muchos` y `uno-a-uno`_ _a la entidad `User`, pero la otra entidad **debe** ser propietaria de la relación \(un `Team `\(equipo\) puede tener una relación `muchos-a-muchos` a `User `pero solo el equipo puede añadir/eliminar usuarios y un usuario no puede añadir/eliminar un equipo\). En la interfaz gráfica del cliente \(generada en AngularJS\) podrás seleccionar un `User `en un _multi-select box_.

## Una relación bidireccional _uno-a-muchos_

Comencemos con dos entidades, un `Owner `\(propietario\) y un `Car`_. _Un propietario puede tener muchos carros y un carro puede tener solo un propietario. 

Así que por un lado,  ésta es una simple relación `uno-a-muchos` \(un propietario tiene muchos carros\), y por otro lado es  una relación `muchos-a-uno` \(muchos carros tiene un propietario\):

`Owner (1) <-----> (*) Car`

Primero crearemos a `Owner`. A continuación se muestran las preguntas relevantes de JHipter respecto a `Owner`:

```
jhipster entity Owner
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Car
? What is the name of the relationship? car
? What is the type of the relationship? one-to-many
? What is the name of this relationship in the other entity? owner
```

Por favor nota que seleccionamos las opciones por defecto respecto a los nombres de las relaciones.

Ahora podemos generar el `Car`:

```
jhipster entity Car
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Owner
? What is the name of the relationship? owner
? What is the type of the relationship? many-to-one
? When you display this relationship with AngularJS, which field from 'Owner' do you want to use? id
```

Se puede hacer lo mismo usando el JDL que se muestra a continuación:

```
entity Owner
entity Car

relationship OneToMany {
  Owner{car} to Car{owner}
}
```

!Eso es todo, ahora tienes una relación `uno-a-muchos` entre estas dos entidades! En la interfaz gráfica de AngularJS tendrás un dropdown en `Car `para seleccionar un `Owner`.

## Una relación unidireccional _muchos-a-uno_

En el ejemplo anterior tenemos una relación bidireccional: a partir de una instancia de `Car `podrías encontrar su propietario, y a partir de una instancia de `Owner `podrías obtener todos sus carros.

Una relación unidireccional `muchos-a-uno` significa que los carros conocen a su propietario pero no al revés.

```
Owner (1) <----- (*) Car
```

Harías estas relaciones por dos razones:

* Desde un punto de vista de negocio, solo usas tus entidades de esta manera. Por tanto no quieres tener una API que permita a los desarrolladores hacer algo que no tenga sentido.
* Tienes una pequeña ganancia de rendimiento cuando usas la entidad `Owner `\(porque no tiene que manejar la colección de carros\).

En ese caso, aún crearías primero la entidad `Owner`, pero esta vez sin relación:

```
jhipster entity Owner
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? No
```

Y después la entidad `Car`, como en el ejemplo anterior:

```
jhipster entity Car
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Owner
? What is the name of the relationship? owner
? What is the type of the relationship? many-to-one
? When you display this relationship with AngularJS, which field from 'Owner' do you want to use? id
```

Esto funcionará como en el ejemplo anterior, pero no podrás añadir o eliminar carros desde la entidad `Owner`. Del lado de la interfaz en AngularJS tendrás un _dropdown _en Car para seleccionar a un `Owner`. Éste es el JDL correspondiente:

```
entity Owner
entity Car

relationship ManyToOne {
  Car{owner} to Owner
}
```

## Una relación unidireccional _uno-a-muchos_

Una relación unidireccional `uno-a-muchos`_ _significa que la instancia `Owner `puede obtener su colección de carros, pero no al revés. Es lo contrario al ejemplo anterior.

```
Owner (1) -----> (*) Car
```

Por lo pronto, este tipo de relación no es proporcionada por defecto en JHipster, mira [\#1569 ](https://github.com/jhipster/generator-jhipster/issues/1569)para más información. 

Tienes dos soluciones para esto:

* Hacer un mapeo bidireccional y usarlo sin modificación: es nuestro enfoque recomendado, porque es más simple.
* Hacer un mapeo bidireccional y luego modificarlo para transformarlo en un mapeo unidireccional:
  * Elimina el atributo "mappedBy" en tu anotación`@OneToMany`
  * Genera la tabla _join_ necesario: puedes ejecutar `mvn liquibase:diff` para generar dicha tabla, mira la [documentación acerca del uso del Liquibase diff](http://www.jhipster.tech/development/). 

Esto no tiene soporte en JDL ni en JHipster.

## Dos relaciones uno-a-muchos en las mismas dos entidades

Para este ejemplo, una `Person `puede ser el dueño de varios carros y también puede ser el chofer de muchos carros:

```
Person (1) <---owns-----> (*) Car
Person (1) <---drives---> (*) Car
```

Para esto necesitamos usar los nombres de las relaciones, los cuales hemos dejado sus valores por defecto en los ejemplos anteriores.

Genera la entidad `Person`, la cual tiene dos relaciones `uno-a-muchos` hacia la entidad `Car`:

```
jhipster entity Person
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Car
? What is the name of the relationship? ownedCar
? What is the type of the relationship? one-to-many
? What is the name of this relationship in the other entity? owner
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Car
? What is the name of the relationship? drivedCar
? What is the type of the relationship? one-to-many
? What is the name of this relationship in the other entity? driver
```

Genera la entidad `Car`, la cual usa el mismo nombre de relación que se configuró en la entidad `Person`:

```
jhipster entity Car
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Person
? What is the name of the relationship? owner
? What is the type of the relationship? many-to-one
? When you display this relationship with AngularJS, which field from 'Person' do you want to use? id
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Person
? What is the name of the relationship? driver
? What is the type of the relationship? many-to-one
? When you display this relationship with AngularJS, which field from 'Person' do you want to use? id
```

Se puede hacer lo mismo usando el JDL siguiente

```
entity Person
entity Car

relationship OneToMany {
  Person{ownedCar} to Car{owner}
}

relationship OneToMany {
  Person{drivedCar} to Car{driver}
}
```

Un `Car `ahora puede tener un chofer y un propietario, los cuales ambos son entidades `Person`. Del lado de la interfaz gráfica de AngularJS tendrás _dropdowns_ en `Car `para seleccionar una `Person `para el campo `owner `y el campo `driver`.

## Una relación _muchos-a-muchos_

Un `Driver `puede tener muchos carros, y un `Car `puede tener muchos choferes.

```
Driver (*) <-----> (*) Car
```

A nivel de base de datos, esto significa que tendremos una tabla join entre las tablas `Driver `y `Car`.

Para JPA, una de estas dos entidades necesitarán administrar la relación: en nuestro caso, esa sería la entidad `Car`, la cual será responsable de añadir o eliminar choferes. 

Generemos el lado no-propietario de la relación, el `Driver`, con una relación _muchos-a-muchos:_

```
jhipster entity Driver
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Car
? What is the name of the relationship? car
? What is the type of the relationship? many-to-many
? Is this entity the owner of the relationship? No
? What is the name of this relationship in the other entity? driver
```

Después genera a `Car`, con el lado propietario de la relación _muchos-a-muchos_:

```
jhipster entity Car
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Driver
? What is the name of the relationship? driver
? What is the type of the relationship? many-to-many
? Is this entity the owner of the relationship? Yes
? When you display this relationship with AngularJS, which field from 'Driver' do you want to use? id
```

Se puede hacer lo mismo usando el siguiente JDL:

```
entity Driver
entity Car

relationship ManyToMany {
  Car{driver} to Driver{car}
}
```

!Eso es todo, ahora tienes una relación _muchos-a-muchos_ entre estas dos entidades! En la interfaz gráfica en AngularJS tendrás un multi-select dropdown en `Car `para seleccionar múltiples `Driver `puesto que Car es el lado propietario. 

## Una relación _uno-a-uno_

Siguiendo nuestro ejemplo, una relación _uno-a-uno_ significaría que un `Driver `puede manejar solo un `Car `y un `Car `puede tener solo un `Driver`.

```
Driver (1) <-----> (1) Car
```

Creemos el lado no-propietario de la relación, en nuestro caso el `Driver`:

```
jhipster entity Driver
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Car
? What is the name of the relationship? car
? What is the type of the relationship? one-to-one
? Is this entity the owner of the relationship? No
? What is the name of this relationship in the other entity? driver
```

Después genera a `Car`, la cual es dueña de la relación:

```
jhipster entity Car
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Driver
? What is the name of the relationship? driver
? What is the type of the relationship? one-to-one
? Is this entity the owner of the relationship? Yes
? What is the name of this relationship in the other entity? car
? When you display this relationship with AngularJS, which field from 'Driver' do you want to use? id
```

Se puede hacer lo mismo con el siguiente JDL

```
entity Driver
entity Car

relationship OneToOne {
  Car{driver} to Driver{car}
}
```

!Eso es todo, ahora tienes una relación _uno-a-uno_ entre estas entidades! En la interfaz gráfica en AngularJS tendrás un dropdown en `Car `para seleccionar un `Driver `puesto que `Car `es el lado propietario.

## Una relación unidireccional _uno-a-uno_

Una relación unidireccional _uno-a-uno_ significa que la instancia `citizen (ciudadano)`puede obtener su pasaporte, pero la instancia de `passport (pasaporte)`no puede obtener a su propietario.

```
Citizen (1) -----> (1) Passport
```

Primero genera la entidad `Passport`, sin ninguna relación con su propietario:

```
jhipster entity Passport
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? No
```

Después, genera la entidad `Citizen`:

```
jhipster entity Citizen
...
Generating relationships with other entities
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? Passport
? What is the name of the relationship? passport
? What is the type of the relationship? one-to-one
? Is this entity the owner of the relationship? Yes
? What is the name of this relationship in the other entity? citizen
? When you display this relationship with AngularJS, which field from 'Passport' do you want to use? id
```

Después de hacer esto, un `Citizen `posee un pasaporte, pero no se define ninguna instancia de `citizen `en `Passport`. En la interfaz gráfica de AngularJS tendrás un _dropdown _en `Citizen `para seleccionar un `Passport `puesto que `Citizen `es el lado propietario. Éste es el JDL correspondiente:

```
entity Citizen
entity Passport

relationship OneToOne {
  Citizen{passport} to Passport
}
```



