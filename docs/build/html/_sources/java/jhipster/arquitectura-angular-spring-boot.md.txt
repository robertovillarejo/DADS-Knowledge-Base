# Documentación del Proyecto Angular 1.5.8 con Spring Boot

El proyecto está dividio en dos componentes, el primero es el `essence-core` y contiene todas las interfaces del meta-modelo de essence y el segundo es el `sekc` que representa al proyecto general. Los repositorios se encuentra en las siguientes url's:

1. [Essence Meta-Model](https://github.com/danimaniarqsoft/essence-metamodel)
2. [Software Engineering Knowledge Composer](https://github.com/dads-software-brotherhood/sekc)


# Instalación de Herramientas

## Herramientas Para trabajar con Angular

Para un mejor uso del proyecto, se recomienda instalar lo siguiente:

1. [Node.js][]: Para la gestión de paquetes en Angular.
2. [Yarn][]: Para la gestión de dependencias.

Después de instalar node, se podrá ejecutar los siguientes comandos.
Primero se deberán de instalar las dependencias en [package.json](package.json).

    yarn install

después, se deberá de instalar Gulp para llevar a cabo operaciones de injección de código:

    yarn global add gulp-cli

[Bower][] Es utilizado para administrar las dependencias CSS y Java Script

### Utilizar Docker para desarrollo

El proyecto puede utilizar varios servicios externos como lo pueden ser bases de datos relacionales, base de datos no relacionales, servidores de búsqueda entre otros. Para facilitar el desarrollo e implementación se recomienda utilizar las imagenes Dockers de dichos servicios. Todas las imagenes Docker de dichos servicios se encuentran en la ruta `[proyecto]/src/main/docker/`.

Por ejemplo, para utilizar la base de datos mysql con docker:

    docker-compose -f src/main/docker/mysql.yml up -d

Para utilizar el elasticsearch:

    docker-compose -f src/main/docker/elasticsearch.yml up -d

Para utilizar sonar:

    docker-compose -f src/main/docker/elasticsearch.yml up -d


_Nota: Para utilizar la imagenes Docker es necesario utilizar [Docker Compose][]_


## Para producción

Para optimizar la aplicación en producción, se deberá de ejecutar el siguiente comando:

    mvn -Pprod clean package

El comando anterior minifica los archivos css y javascript. También modifica el archivo `index.html` para utilizar los archivos optimizacos.

Para asegurarnos que todo está funcionando bien, se deberá de ejcutar el siguiente comando:

    java -jar target/*.war

La aplicación se deberá de desplegar en la siguiente URL [http://localhost:8080](http://localhost:8080).


1. Instalar Mongodb
2. Instalar Java 8
3. Instalar Maven
4. Instalar Eclipse última versión web.


# Configuración del proyecto

## Clonar el proyecto del prepositorio de git
```bash
git clone -b develop git@github.com:dads-software-brotherhood/sekc.git 
```

## Levantar el proyecto
```bash
mvn spring-boot:run
```

el proyecto estárá disponible en la url:
```bash
http://localhost:8080
```

# Estructura del proyecto

```
webapp
├── app                               - Your application
│   ├── account                       - User account management UI
│   ├── admin                         - Administration UI
│   ├── blocks                        - Common building blocks like configuration and interceptors
│   ├── components                    - Common components like alerting and form validation
│   ├── entities                      - Generated entities (more information below)
│   ├── home                          - Home page
│   ├── layouts                       - Common page layouts like navigation bar and error pages
│   ├── services                      - Common services like authentication and user management
│   ├── app.constants.js              - Application constants
│   ├── app.module.js                 - Application modules configuration
│   ├── app.state.js                  - Main application router
├── bower_components                  - Dependencies installed by Bower
├── content                           - Static content
│   ├── images                        - Images
│   ├── styles                        - CSS stylesheets
│   ├── fonts                         - Font files will be copied here
├── i18n                              - Translation files
├── scss                              - Sass style sheet files will be here if you choose the option
├── swagger-ui                        - Swagger UI front-end
├── 404.html                          - 404 page
├── favicon.ico                       - Fav icon
├── index.html                        - Index page
├── robots.txt                        - Configuration for bots and Web crawlers
```




[Node.js]: https://nodejs.org/
[Yarn]: https://yarnpkg.org/
[Bower]: http://bower.io/
[Docker Compose]: https://docs.docker.com/compose/

