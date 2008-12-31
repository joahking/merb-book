# Estructura del proyecto

* Esto será una tabla de contenidos (este texto será pegado).
{:toc}

La estructura típica de una aplicación Merb nueva
(generada con ``merb-gen app``)
lucirá más o menos así:

    Directorio de la aplicación (Merb.root)
      - app
        - controllers
        - helpers
        - models
        - views
          - exceptions
          - layout
      - autotest
      - config
        - environments
      - doc
      - gems
      - merb
      - public
        - images
        - javascripts
        - stylesheets
      - spec
      - tasks

Hagamos un breve resumen de cada directorio y su función.

## app
Este directorio es donde emplearás la mayor parte de tu tiempo,
dado que contiene las "entrañas" de tu aplicación Merb.

### controllers
Todos los controladores de tu aplicación son almacenados aquí
(esto no supone sorpresa).
Los controladores son típicamente nombrados en plural.
Por ejemplo, si tienes un modelo "``Page``",
el archivo del controlador normalmente se llamará ``pages.rb``.
Esto es simplemente una convención, al fin y al cabo;
tú eres libre de nombrar tus controladores como más te guste.
Visita la sección [controladores][] para más información.

### models
Este directorio contiene las clases de tus modelos.
Estas clases típicamente sirven como tu [ORM][],
que proveen acceso orientado a objecto a tus tablas en la base de datos.
Visita la sección [modelos][] para más información.

### views
Cualquier plantilla para las vistas se almacenarán aquí.
Por defecto, este directorio contiene los subdirectorios ``exceptions`` y
``layout``.
El directorio ``exceptions`` almacena plantillas que están generalmente
relacionadas a errores HTTP.
Por ejemplo, una aplicación Merb nueva contendrá una vista
``not_found.html.{erb,haml}``,
que corresponde al código de estado 404 de HTTP.
El directorio ``layout`` contiene las plantillas sobretodo de la aplicación,
dentro de las que las plantillas de las acciones pueden ser mostradas.
El archivo layout por defecto de la aplicación se llama
``application.html.{erb,haml}``.
Visita la sección [vistas][] para más información.

## config
Si, lo adivinaste.
Los archivos de configuration de Merb son guardados aquí.
El archivo ``router.rb`` contiene las [rutas](/getting-started/router) URL de
tu aplicación, que definen la estructura, ordednr, y apariencia de tus URLs.
Otro archivo importante, ``init.rb``,
se encarga de la configuration base de Merb.
Aquí es donde puedes configurar tu ORM, motor de plantillas, y plataforma de
pruebas.
También puedes adicionar configuraciones a medida al ``Merb::BootLoader`` en
sus bloques ``before_app_loads`` y ``after_app_loads``.
Otro archivo importante, ``dependencies.rb``, es donde puedes defindire las
dependencias de tu aplicación: otras librarias o gemas que tu aplicación requiere.
Cualquire dependencia listada en ese archivo será cargada cuando tu aplicación
Merb arranca.

### environments
Aquí es donde cualquier archivo de configuración específico a algun entorno son
guardados.
Aquí existen un par de archivos típicos de configuración (en Ruby puro); cada
uno corresponde a un entorno (desarrollo, producción, etc.) específico de Merb.

## gems
Cuando estés listo para [desplegar][] tu aplicación, es recomendable que
[empaquetes][] todos tus dependencias dentro del directorio de la aplicación.
El directorio ``gems`` es donde estas dependencias empaquetadas serán
almacenadas.
Cuando inicies la aplicación Merb, esta cargará cualquier gema desde este
directorio, priorizándolas a las gemas del sistema.

## public
Aquí es donde puedes almacenar  archivos "estáticos", tales como los archivos
``favicon.ico`` y ``robots.txt``.

### images
Cualquier imagen que tus plantillas pueden usar deberán ir aquí.

### javascripts
En la "Merb stack" típica, este directorio contiene dos archivos:
``application.js`` y ``jquery.js``.
La Merb stack por defecto viene empaquetada con la poderosa plataforma
de JavaScript [jQuery][].

Si tienes cantidades pequeñas de código JavaScript específico a tu aplicación,
deberá ir dentro del archivo ``application.js``.
Si resulta que esto se demuestra inviable,
puedes adicionar más archivos según necesites.

### stylesheets
Siguiendo la idea de la separación de intereses, cualquier estilismo visual
deberá ser puesto en una hoja de estilos.
Cuando generás una aplicación Merb, se crea un archivo de hoja de estilos
(llamado ``master.css``), que puedes ajustar a tus deseos.

## spec
Si estás usando [rspec][] como tu plataforma de [pruebas][], este directorio
contendrá estos tests.
Por defecto, este directorio contiene dos archivos: un archivo ``spec.opts``
vacío, que puedes usar para adicionar argumentos por linea de comandos a rspec
(añadir salida coloreada, etc.)
y ``spec_helper.rb``, que es donde puedes ajustar a conveniencia el
spec runner que utilices (entre otras cosas).

## tasks
Este directorio contiene las tareas de [thor][] de Merb.


<!-- Links -->
[empaquetar]:      /deployment/bundle
[controladores]:   /getting-started/controllers
[desplegar]:       /deployment
[jQuery]:          http://jquery.com/
[modelos]:         /getting-started/models
[ORM]:             http://en.wikipedia.org/wiki/Object-relational_mapping
[rutas]:           /getting-started/router
[RSpec]:           http://rspec.info/
[probando]:        /testing-your-application
[thor]:            http://wiki.merbivore.com/faqs/thor
[vistas]:          /getting-started/views
