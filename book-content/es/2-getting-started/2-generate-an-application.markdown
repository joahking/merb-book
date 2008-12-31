# Generar un aplicación

* Esto será una tabla de contenidos (este texto será pegado).
{:toc}

Merb viene con un generador (``merb-gen``) para crear aplicaciones Merb.
El generador puede generar diferentes tipos aplicaciones Merb,
para ver todas las opciones del generador usa

    $ merb-gen -H
{:lang=shell html_use_syntax=true}

Por ahora, limitémonos solamente a los tipos de aplicaciones Merb que se pueden
generar.

## Tipos
Merb puede ser usado para desarrollar desde aplicaciones consistentes en un solo
archivo, muy pequeñas y rápidas hasta grandes y complejos web services.
Diferentes estructuras para la aplicación pueden generarse,
dependiendo de las necesidades del desarrollador.
Las diferentes estructuras para la aplicación que puedes generar son ``app``
para una stack, ``core`` para un núcleo, ``flat`` para aplicaciones planas, y
``very_flat`` para aplicaciones muy planas.

### App (stack)
El stack dogmático de Merb.
Esto genera una estructura de directorios completa para la aplicación con un
conjunto completo de archivos de configuración.
También adiciona un archivo ``config/dependancies.rb`` que incluye todo el
``merb-more`` y ``DataMapper``.

    $ merb-gen app my-application
{:lang=shell html_use_syntax=true}

Esta aplicación tiene todo lo necesario para comenzar a construir una aplicación
web completa; es la más similar a la estructura típica de Rails.
Mucho en este libro funcionará con la suposición de que has comenzado de esta
manera.

Arranca esta aplicación ejecutando ``merb`` en el directorio raiz de la
aplicación.
Esto iniciará Merb y lo pondrá a escuchar en el puerto por defecto 4000.
Para ver tu aplicación en acción, visita [http://localhost:4000/][].

### Core (núcleo)
Core generará una estructura de directorios completa para la aplicación con un
conjunto
completo de archivos de configuración.
A diferencia de ``app`` -- la stack completa dogmática -- ninguna dependencia se
adicionará.

    $ merb-gen core my-apliccation
{:lang=shell html_use_syntax=true}

Arranca esta aplicación ejecutando ``merb`` en el directorio raiz de la
aplicación.
Fíjate que, a diferencia de las otras tres aplicaciones generadas,
en core no existe contenido por defecto.
Visitando [http://localhost:4000/][] generará un error hasta que adiciones
contenido y enrutamiento.

### Flat (plana)
Una aplicación plana mantiene toda su lógica en un solo archivo, pero tiene
archivos separados para configuración y un directorio propio para las vistas.

    $ merb-gen flat my-application
{:lang=shell html_use_syntax=true}

Las aplicaciones planas son arrancadas, como cualquier otra aplicación merb
generada,
ejecutando ``merb`` en el directorio raiz de la aplicación.
Por defecto, todos los métodos en la clase ``my-application`` serán tratados
como acciones con ``my-application`` como el controlador. e.g.:
[http://localhost:4000/my-application/foo][]

Esto invocará ``MyApplication#foo`` y mostrará la salida desde la plantilla
``foo.html.*``.

### Very Flat (muy plana)
A aplicación muy plana es similar a otros micro frameworks de Ruby, donde la
aplicación completa esta en un solo archivo.

    $ merb-gen very_flat my-application
{:lang=shell html_use_syntax=true}

Para arrancar una aplicación muy plana, inicia Merb con el siguiente comando
(en el directorio de tu aplicación):

    $ merb -I my-application.rb
{:lang=shell html_use_syntax=true}

Esto arrancará  Merb y lo pondrá a escuchar en el puerto por defecto (4000).
Para ver tu aplicación en acción, visita [http://localhost:4000/][].

[http://localhost:4000/]:     http://localhost:4000/
[http://localhost:4000/my-application/foo]: http://localhost:4000/my-application/foo
