# Introducción a BDD (Behaviour Driver Development) con Cucumber.

> **Autor** : Ivan Rodrigo Nievez Mondragon  
> **Correo**: ivan.nievez@infotec.mx

#### ¿Qué es BDD?
*BDD* es un proceso de desarrollo software, si bien se relaciona principalmente con el Testing, que es de donde surge, trata de combinar los elementos técnicos y los de negocio, de manera que tengamos un marco de trabajo, y un marco de pruebas, en el que los requisitos de negocio formen parte del proceso de desarrollo, escribiendo primero las pruebsa y desde ahí, el desarrollo.

En *BDD* en lugar de escribir pruenbas unitarias, lo que haremos es escribir, pruebas que verifiquien que el comportamiento del código es correcto desde el punto de vista del negicio. Al terminar, realizaremos el código fuente de la funcionalidad descrita en la prueba, para que estas pasen correctamente.

Para poder hacer esto, partiremos de las *historias de usuario* siguiendo el modelo:
- Como [rol]
- Quiero [ característica | acción | meta ]
- Para que [beneficio | valor | meta]

### ¿Y cómo hacemos estó?

Bueno, ya que necesitamos un  lenguaje que se pueda interpretar por los usuarios, interesados, testers y desarrolladores, utilizaremos *Gherkin*.

*Gherkin* es un lenguaje entendible por el negocio, lo que se llama un DSL (Domain-Specific Language), más o menos cercano al lenguaje natural. Es un lenguaje muy simple. Sólo tienen 5 sentencias:
- **Features:** Indica el nombre de la funcionalidad que vamos a probar. Debe ser un título claro y explícito. Aquí es donde podemos usar el modelo de las *historias de usuario*, sobre esta descripción empezaremos a construir nuestros escenarios de prueba.
- **Scenario:** Describe cada escenario que vamos a probar.
- **Given:** Provee contexto para el escenario en que se va a ejecutar el test, tales como punto donde se ejecuta el test, o prerequisitos en los datos. Incluye los pasos necesarios para poner al sistema en el estado que se desea probar.
- **When:** Especifica el conjunto de acciones que lanzan el test. La interacción del usuario que acciona la funcionalidad que deseamos testear.
- **Then:** Especifica el resultado esperado en el test. Observamos los cambios en el sistema y vemos si son los deseados.

## ¿Y cómo funciona con Cucumber?

*Cucumber* es un framework que nos ayuda a escribir y ejecutar descripciones de alto nivel del funcionamiento de nuestro software y que podemos utilizar para automatizar nuestras pruebas en BDD.
Una de las principales carecteristicas de este framework, es que nos permite escribir estas descripciones en texto plano y en nuestro idioma nativo.

Las descripciones de nuestras *historias de usuario* se guardan en archivos *.features* escritos en el lenguaje *Gherkin*. A continuacion mostraremos un ejemplo.

```Gherkin
Feature: Como Cliente,
         quiero retirar efectivo de un ATM
         para que no tenga que esperar en la fila en el banco

 Scenario: se desea retirar dinero con una cuenta solvente
    Given la Cuenta es solvente
    And el ATM contiene efectivo
    When el Cliente solicita retirar dinero
    Then el sistema se asegura que la cuenta tiene dinero
    And se asegura que el efectivo es entregado
    And se asegura que la tarjeta es regresada

 Scenario: se desea retirar dinero pero la cuenta no tiene solvencia
    Given la cuenta esta sobregirada
    And la tarjeta es valida
    When el Cliente solicita retirar dinero
    Then el Sistema se asegura de mostrar al Cliente un mensaje en el que se le indica que no es posible retirar dinero
    And se asegura que el dinero no es entregado
```

**Nota:** *Gherkin* tiene 5 sentencias para su uso, pero en el segundo escenario en la última línea podemos notar el uso de la palabra *And*, esta palabra puede ser utilizada en cualquier parte de la descripción del escenario para evitar repetir (Given, When, Then).

Descripción del ejemplo completo:
https://gist.github.com/danimaniarqsoft/640d512c3b183fdef1a2856354c83068

Fuentes:
https://www.genbetadev.com/metodologias-de-programacion/bdd-cucumber-y-gherkin-desarrollo-dirigido-por-comportamiento
http://www.javiergarzas.com/2014/12/bdd-behavior-driven-development-1.html
https://blog.engineyard.com/2009/cucumber-introduction
