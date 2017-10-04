==============
Contribuciones
==============

Publicar un artículo en la base de conocimientos
================================================

1. Crear el archivo con extensión ``.rst`` o ``.md``

    Buscar o crear la carpeta donde poner el archivo según el tema. Por ejemplo, ``source/como-escribir-articulo.rst``

2. Añadir el archivo recién creado a un índice

    Para que se puede acceder al artículo es necesario incluirlo en algún índice. Esto se logra con la directiva ``toctree``.

    Para incluir este artículo en el índice general es necesario escribir el nombre del archivo (sin extensión) en ``source/index.rst``, por ejemplo:

.. code:: bash

  .. toctree::
      :caption: Contribuciones

      como-escribir-articulo

Para conocer más sobre la directiva toctree, visita http://www.sphinx-doc.org/es/stable/markup/toctree.html
