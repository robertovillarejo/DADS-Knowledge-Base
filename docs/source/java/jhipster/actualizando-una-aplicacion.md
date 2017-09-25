# Actualizando una aplicación

Cuando se lanza una nueva versión de JHipster, el sub-generador de actualización de JHipster ayuda a actualizar la aplicación existente a esta nueva versión, sin borrar tus cambios.

Esto es útil para:

* Tener las últimas características de JHipster en una aplicación existente
* Obtener los cambios cuando haya un _bug fix_ o una actualización de seguridad importante
* Retener tus cambios en tu código base  y fusionarlos fácilmente con código generado recientemente

_Por favor lee cuidadosamente esta página antes de hacer un upgrade, para entender cómo funciona el proceso de actualización._

## Requerimientos

Para que este sub-generador funcione necesitas tener instalado `git `de [http://git-scm.com](http://git-scm.com/).

## Ejecutando el sub-generador

Entra al directorio raíz de la aplicación:

`cd myapplication/`

Para actualizar tu aplicación, escribe:

`jhipster upgrade`

Aquí están las opciones que puedes pasar:

* `--verbose` - Hace un log detallado de cada paso del proceso de actualización
* `--target-version=4.2.0` - Actualiza a la versión especificada en lugar del último _release, _útil si un proyecto está atrasado muchas versiones
* `--force` - Ejecuta el sub-generador upgrade aún si no hay ninguna versión de JHipster disponible

## Vista gráfica del proceso de actualización

Aquí está cómo funciona el proceso de actualización gráficamente \(lee las secciones de abajo para tener una explicación textual\):

![](http://www.jhipster.tech/images/upgrade_gitgraph.png)\(Esta imagen proviene de [JSFiddle](http://jsfiddle.net/lordlothar99/tqp9gyu3/)\)

Por favor nota que la rama `jhipster_upgrade` será creada huérfana en tu proyecto, aunque no se muestra correctamente en el gráfico de arriba.

## Explicación del proceso de actualización paso por paso

A continuación se muestran los pasos procesados por el sub-generador _upgrade _de JHipster:

1. Revisar si hay una nueva versión disponible de JHipster \(no aplicable si estás usando `--force`\)
2. Revisar si la aplicación ya ha sido inicializada como un repositorio git, o en caso contrario inicializará uno por ti y hará un _commit_ el código base actual a la rama _`master`_.
3. Asegurar que no hay cambios locales sin guardar \(_commit_\) en el repositorio. El proceso se abortará si se encuentran cambios sin guardar.
4. Revisa si existe una rama `jhipster_upgrade`. Si no, se crea una rama: los detalles sobre este paso se proporcionan en la sección "Pasos específicos ejecutados en la primera actualización".
5. Se mueve a la rama `jhipster_upgrade`
6. Actualiza de manera global a JHipster al la última versión disponible. 
7. Limpia el directorio del proyecto actual.
8. Re-genera la aplicación usando el comando `jhipster --force --with-entities`.
9. Hace _commit _del código generado a la rama `jhipster_upgrade`.
10. Fusiona la rama `jhipster_upgrade `con la rama original desde donde se ejecutó el comando `jhipster upgrade`.
11. Ahora solo necesitas proceder a resolver los conflictos del _merge_ si es que hay.

!Felicidades, ahora tu aplicación está actualizada con la última versión de JHipster!

## Pasos específicos ejecutados en la primera actualización

En la primera ejecución del sub-generador _upgrade_ de JHipster, a fin de evitar el borrado de tus cambios, se ejecutan algunos pasos adicionales:

1. Se crea una rama huérfana `jhipster_upgrade `\(no tiene padre\)
2. Se genera la aplicación completa \(usando tu versión actual de JHipster\).
3. Se hace un _block-merge_ en la rama master: no se hacen alteraciones en tu código base en la rama master; ésta es solo una forma práctica de grabar en Git que el HEAD de `master `está actualizado con la versión actual de JHipster. 

## Consejo

No hagas _commit_ en la rama `jhipster_upgrade`. Esta rama está dedicada al sub-generador _upgrade_ de JHipster: cada vez que se ejecuta el sub-generador, se crea un nuevo _commit_. 



