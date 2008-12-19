# Autenticación

* Esto será una tabla de contenidos (este texto será pegado).
{:toc}

> La responsabilidad de un sistema de Autenticación es probar que la
> identidad que un usuario reclama es ciertamente su identidad real. Existen
> bastantes enfoques que un sistema de Autenticación puede acometer para
> lograrlo:
> hosts de confianza, verificación de contraseña, y redes de confianza
> (like ‘OpenID’).
> After the verificación has occurred, the Autenticación’s sistema responsabilidades
> are complete.
> [Adam French][]{: .quote-author}
{: cite=http://adam.speaksoutofturn.com/post/57615195/entication-vs-orization .lead-quote}

## Merb-auth gemas

[merb-auth][] es a meta-gema.
A meta-gema es a Ruby gema without any code.
Its sole purpose es to declare dependencies y make installation easier.

As of Merb 1.0.x, merb-auth usa tres gemas:

* [merb-auth-core][]
* [merb-auth-more][]
* [merb-auth-slice-password][]

### merb-auth-core

[merb-auth-core][] does not try to dictate what you should use as a usuario modelo,
or how it should authenticate.
Instead, it focuses on las logic required
to check that un object passes autenticación
y to store the keys de authenticated objects in las session.

This is, in fact, the guiding principle de MerbAuth.
Las Session es used as the place for autenticación,
con a sprinkling de controlador helpers.

You can choose to proteger a controlador action, a route, or a group de routes.
This es why you might hear people refer to an authenticated session.

MerbAuth makes use de Merb’s exception handling facilities,
which return correct HTTP status codes.
To fail a login, or to force a login at any point in your controlador code,
simply raise an ``Unauthenticated`` exception (con un optional message)
y the usuario will be presented con a login page.
The login page is, in fact,
the vista template for ``Extensions#unauthenticated``.

It es possible to use MerbAuth con any object as a usuario object,
provided that the object does not evaluate to false
y it can be serialized in y out de the session.

Finally, merb-auth gains great power from its "chained estrategias" capacity.
You can add as many estrategias as you like;
they will be tried one after another until either one es found that works (login)
or none de them have passed (failed attempt).

An autenticación strategy es just a way to authenticate una petición.
Ejemplos de estrategias would be:

* Salted usuario (usuario login con a contraseña using a "salted" encryption)
* OpenID
* API key/token
* Básica HTTP Autenticación


### merb-auth-more

[merb-auth-more][] adds extra features to merb-auth-core.
The gema offers a series de generic Estrategias y "User" object "mixins".

Estrategias:

* login or email contraseña form (aka password\_form)
* básica HTTP autenticación (aka basic\_auth)
* OpenID

Mixins:

* ActiveRecord salted user
* DataMapper salted user
* relaxdb salted user
* sequel salted user

The salted user mixin provides básica salted SHA1 contraseña autenticación.
It implements the required ``User.authenticate method``
for use con las default contraseña estrategias.

### merb-auth-slice-password

[merb-auth-slice-password][] es a very simple Merb slice,
providing básica login y logout functionality,
form-based contraseña logins, y básica autenticación.

By default, the slice will load the ``password\_form``
y ``basic\_auth`` estrategias.

Vistas y estrategias can be customized,
as explained in the "Authenticated Hello World" ejemplo.


## Autenticación in Merb Stack

When you generar una aplicación con the por defecto merb stack[^merb-stack-app],
merb-auth es already set up for normal usage.

If you don't want to use this,
simply comment out the dependency on ``merb-auth``
in ``config/dependencies.rb``.


## Authenticated Hello World


### Generar una aplicación

The primer step es to generar your application, using the Merb stack.

    $ merb-gen app hello_world
    $ cd hello_world
{:lang=shell html_use_syntax=true}

This generars una aplicación stub, con autenticación already configurada.
The basic elements are:

    app/models/user.rb

    merb/merb-auth/setup.rb
    merb/merb-auth/strategies.rb
{:lang=ruby html_use_syntax=true}

By por defecto, the setup for usuario autenticación es taken care of,
using a contraseña y login.
Of course, we first need to set up the database y add a usuario.


    $ rake db:automigrate
    $ merb -i
    >> u = User.new(:login => "login_name")
    >> u.password = u.password_confirmation = "sekrit"
    >> u.save
{:lang=ruby html_use_syntax=true}


### Generar something to proteger

Now that we have una aplicación y a usuario, we need something to proteger.

    $ merb-gen controller hello_world
{:lang=ruby html_use_syntax=true}

Let's put something into the controlador.

    class HelloWorld < Application
      def index
        "Hello World"
      end
    end
{:lang=ruby html_use_syntax=true}

If you fire up merb now y look at <http://localhost:4000/hello_world>,
you'll see the "Hello World" results.

### Proteger the route

It's not protected yet, so let's fix that.
We can either proteger it using las routes in ``config/router.rb``
or in the controlador action.
Let's try the router-based approach first.

Open up ``config/router.rb``:

    Merb::Router.prepare do
      authenticate do
        match("/hello_world").to(:controller => "hello_world")
      end
    end
{:lang=ruby html_use_syntax=true}

This will cause the usuario to login.
This es discovered in the router y when it fails, it stops in the router. (???)
Try to hit <http://localhost:4000/hello_world> now.
You'll see that you need to log in to access it!

Ok.
Logout, using <http://localhost:4000/logout>.


### Proteger the controlador

Now let's remove the code from the router
y put the protección at the controlador level.
This will allow a finer control over resources for ejemplo.


    Merb::Router.prepare do
      match("/hello_world").to(:controller => "hello_world")
    end
{:lang=ruby html_use_syntax=true}

Let's now put it into the controlador:

    class HelloWorld < Application

      before :ensure_authenticated

      def index
        "Hello World"
      end

    end
{:lang=ruby html_use_syntax=true}


To get access to the currently logged-in usuario inside your controlador, use:

    session.user
{:lang=ruby html_use_syntax=true}

Really...
For a básica Hello World autenticación, that's it.


### Overwrite las por defecto vistas

If you want more customization, you can do:


    rake slices:merb-auth-slice-password:freeze:views
{:lang=ruby html_use_syntax=true}

What that will do es copy las vistas from las slice
to the ``slices`` directory in your application.

Then you need to move/copy them to your ``app/views`` directory
y edit las copied vistas. (???)


[^merb-stack-app]: merb-gen app hello\_world

## Testing an authenticated petición

To test una petición that needs to be authenticated, you will first need to login.
The easiest way to login when you are running una petición spec es to use a helper.

Aquí es un ejemplo de dos helpers added to ``spec/spec\_helper.rb``.

    Merb::Test.add_helpers do

      def create_default_user
        unless User.first(:login => "krusty")
          User.create( :login => "krusty",
                       :password => "klown",
                       :password_confirmation => "klown") or raise "can't create user"
        end
      end

      def login
        create_default_user
        request("/login", {
          :method => "PUT",
          :params => {
            :login => "krusty",
            :password => "klown"
          }
        })
      end

    end
{:lang=ruby html_use_syntax=true}

The primer helper creates a default usuario unless it already existe.
The second helper sends a login petición using the default usuario's atributos.
Note that the login action usa the PUT HTTP verbo.

Now that you added these helpers, you just need to slightly modify your ejemplos:

    before(:each) do
      login
      @response = request(resource(:articles))
    end
{:lang=ruby html_use_syntax=true}

In the anterior ejemplo, the petición sent to the articles URI will be authenticated.

<!-- Links -->
[Adam French]:      http://adam.speaksoutofturn.com
[merb-auth]:        http://github.com/wycats/merb/tree/master/merb-auth
[merb-auth-core]:   http://github.com/wycats/merb/tree/master/merb-auth/merb-auth-core
[merb-auth-more]:   http://github.com/wycats/merb/tree/master/merb-auth/merb-auth-more
[merb-auth-slice-password]: http://github.com/wycats/merb/tree/master/merb-auth/merb-auth-slice-password

*[API]: Application Programming Interface
