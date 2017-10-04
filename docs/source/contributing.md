# Contribuciones

En este artículo se describen los lineamientos para contribuir con documentación, tutoriales, guías, etc. a la base de conocimientos de la DADS.

La base de conocimientos de la DADS está construida sobre Sphinx que es la tecnología que genera archivos htlm a partir de archivos escritos en:

- [reStructuredText](http://docutils.sourceforge.net/docs/user/rst/quickref.html)
- [Markdown](https://guides.github.com/features/mastering-markdown/)

## Añadir un nuevo artículo

A fin de mantener un orden en la base de conocimientos, cuando se escribe un artículo de un tema nuevo, entonces debe crearse una carpeta con el nombre del tema y colocar el nuevo artículo dentro.  

Por ejemplo, se desea contribuir con un artículo sobre cómo instalar docker en Linux y Windows.
Como es muy probable que en el futuro se agreguen más artículos sobre docker, se creará una carpeta llamada docker que contenga el artículo y un índice.

```
docs                                
├── source                            - Artículos en rst o md
|   ├── docker                        - Aquí van todos los artículos sobre Docker
|       ├── instalar-docker.md        - Artículo sobre la instalación de Docker
|       ├── index.rst                 - Índice de todos los artículos sobre Docker
|   ├── index.rst                     - Índice de toda la documentación de la base de conocimientos
|   ...                               - Otros artículos generales
```

En los siguientes archivos:
- La directiva `toctree` crea un árbol de contenido
- `maxdepth` indica el nivel máximo de títulos a mostrar en el árbol de contenido

El archivo `docs/source/docker/index.rst` existe con el fin de crear el árbol de contenido de los artículos relaciondos exclusivamente con Docker:

```
Docker
======

..  toctree::
  :maxdepth: 2

  instalar-docker
```

---

Al archivo `docs/source/index.rst` se le agrega un *capítulo* Docker y se indica la ruta del índice de Docker:

```
!Base de conocimientos de la DADS!
==================================================

.. toctree::
  :maxdepth: 2
  :caption: Docker

  docker/index
```

Para generar los archivos html:

- Borrar la carpeta `docs/build` (recomendable)
- Dentro de la carpeta `docs`, ejecuta `make html`
- Verificar el resultado abriendo `docs/build/htlm/index.html` en cualquier navegador
