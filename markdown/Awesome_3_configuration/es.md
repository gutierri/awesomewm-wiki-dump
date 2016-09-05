---
title: Awesome 3 configuration es
permalink: /Awesome_3_configuration/es/
---

Fondo
=====

**awesome 3** ahora usa un archivo de configuración basado en el lenguaje [Lua](http://www.lua.org). La mayoría de las personas que vienen del mundo de awesome 2 llorarán viendo que el viejo archivo de configuración en el formato libconfuse ha desaparecido, otros odiarán esto porque nunca entendieron el archivo de configuración de [ion](http://modeemi.fi/~tuomov/ion/), pero yo tampoco lo entendí.

¿Por qué Lua? Porque ofrece más control. Si ofrece comportamiento condicional: querías que awesome hiciera algo cuando algo sucediera, no podías hacerlo. Ahora, puedes. Puedes hacer casi **cualquier** cosa (en el rango de lo que un manejador de ventanas puede hacer).

Archivos
========

-   ~/.config/awesome/rc.lua
-   /etc/xdg/awesome/rc.lua

Base de Lua
===========

Estamos hablando acerca de Lua, entonces lo primero, aprende Lua. ¿No quieres? No uses awesome 3 y para de leer ahora. (Alternativamente toma un archivo de configuración del código fuente, o de alguien, y solo modifícalo adecuadamente, lo cual debería funcionar incluso sin ningún conocimiento del lenguaje Lua).

Para las personas que siguen leyendo, ¡excelente! Lua es un lenguaje **simple**. Por otro lado, si no estás familiarizado del todo con los lenguajes de las computadoras, por ejemplo, si no sabes qué son *objetos*, *métodos* y *argumentos*, bueno, estás muy perdido para este documento, ¡aprende algunas cosas básicas y regresa!

No estoy siendo elitista: awesome 3 está diseñado para usuarios poderosos con un mínimo de conocimientos de ciencias de la computación, sin embargo, si estás realmente motivado, puedes aprender lo básico y lo suficiente para configurar y controlar awesome.

La mejor manera de aprender Lua es usar una o dos horas leyendo el libro [Programming In Lua](http://www.lua.org/pil/), el cual es bastante bueno y te dará una descripción general de lo que es posible hacer en Lua. No dudes en agregar a tu bookmark páginas importantes que hablen acerca de control de flujo, declaraciones útiles, entre otros. Eso te ayudará: es horrible buscar por la sintaxis *for* cuando estás tratando de escribir líneas de código.

Tipos de objetos en awesome
===========================

Para la gente que viene de otro espacio que no usó awesome antes, aquí están los objetos básicos de awesome que se te darán y que tendrás que manipular pronto.

Screen
------

No hay un objeto real llamado screen en awesome, pero hablaremos de las screens luego. Un screen es un monitor físico conectado a tu computadora. Ellos estan representados por sus índices, comenzando en 1.

Client
------

Un Client es una ventana. Creo ser claro.

Tag
---

Un tag es algo como un espacio de trabajo/escritorio pero con un concepto menos rígido Cada client tiene al menos un tag asignado a él. Cada Screen tiene al menos un tag. A cualquier tiempo, en un screen, puedes ver cualquier número del tag. El no observar ningún tag ocultará todos los clients los cuales tienen dichos tags.

Widget
------

Los Widgets son objetos que dibujan cosas en el screen. Hay varios tipos de widgets. Por ejemplo, hay un widget caja de texto el cual imprime texto en el screen. También hay un widget caja de icono, el cual dibuja un icono en el screen.

Titlebar
--------

Un titlebar es una barra que está alrededor del client y adjunta a este. Puedes poner widgets en ella.

Statusbar
---------

Un statusbar (barra de estado) es una barra que está fijada al borde de un screen. Puedes colocar widgets en el.

Construyendo nuestra interfaz
=============================

Para construir nuestra interfaz, necesitamos usar los métodos y funciones de awesome: **todo** esta documentado en [Lua API documentation](http://awesome.naquadah.org/apidoc/) y en el manual awesomerc(5). Léelos. Son tu referencia para desarrollar. Repito: Léelo, todo está documentado en ese manual y en luadoc.

¿Cuál manual?… awesomerc(5). Bien. Nos estás siguiendo.

Creación de un Tag
------------------

Si comienzas sin ninguna operación de Lua, tendrás un número de tags igual a cero, Lo cual es fatal. awesome requiere al menos un tag.

Entonces lo primero, necesitamos crear un tag. ¿Cómo hacemos eso? Buscamos por tag() y su documentación, la función del creador de tags en el manual dice que el parámetro de la función debe ser una tabla con al menos un atributo nombre. Hagámoslo:

`mytagone = tag({ name = "one" })`

*mytag* es ahora un objeto tag, y su nombre es *one*. Si queremos configurar un layout por defecto, tenemos que agregarlo como un par llave/valor dentro de la tabla, como se explica en la documentación.

`mytagtwo = tag({name = "two", layout = "floating" })`

Ahora, tenemos un segundo tag: su nombre es *two*, y por defecto ordena los clientes con algoritmos flotantes, los cuales, como su nombre sugiere, dejan flotar los clientes.

Crear objetos es divertido, pero bueno, por ahora son inútiles. Recuerda: cada screen tiene al menos un tag. Entonces necesitamos agregar esos tags creados a nuestro primer screen. El atributo *screen* de el objeto tag hace eso por nosotros, solo necesitamos darle el valor del número del screen.

`mytagone.screen = 1`

Acabamos de agregar ese tag al screen \#1. Si tenemos un segundo screen (multi-head: Zaphod, Xinerama o XRandR son indenticos para awesome), podemos agregar nuestro segundo tag a dicho screen:

`mytagtwo.screen = 2`

Y si tenemos 3 screens, Necesitamos... Bueno, Creo que entiendes.

Si quieres saber cuántas screens tienes, puedes usar la función*screen.count()*.

Puedes manipular la forma las vistas de un tag con el atributo*selected*. Si quieres ver tu tag *one*:

`mytagone.selected = true`

Si quieres ocultarlo, puedes obviamente configurar el valor a *false*.

Puedes manipular todas las configuraciones de los tag con sus atributos. Lee la documentación.

Manipulación de Clientes
------------------------

Algunas veces, crearás objetos client. Esto es bastante útil en funciones de tipo hook, las cuales discutiremos luego.

Un cliente, es un objeto el cual también tiene métodos, como tags entre otros. Nosotros decidimos que la variable *c* es nuestro objeto client.

Por ejemplo, si queremos configurar un cliente flotante (client floating) podemos usar el atributo *floating* como este:

`c.floating = true`

Algunos métodos devuelven valores que pueden ser usados luego para analizar. Si queremos saber si un cliente es flotante (floating), podemos obtener el atributo *floating*:

`is_client_floating = c.floating`

La variable *is_client_floating* ahora contiene un valor booleano: *true* si el cliente es flotante o *false* si el cliente no es flotante.

Podemos combinar esas 2 funciones para crear una función alterna: esta función configurará el estado cliente flotante (client floating) a verdadero (true) si no es, o falso (false) si lo es.

`function client_floating_toggle(c)`
`    if c then`
`        c.floating = not c.floating`
`    end`
`end`

También podemos usar eso para alternar un tag en nuestro cliente:

`function client_tag_with_tag_one(c)`
`    if c then`
`        local t = c:tags()`
`        table.insert(t, one)`
`        c:tags(t)`
`    end`
`end`

Creación de Widget
------------------

Los Widgets son pequeños objetaos que pueden ser colocados ya sea en el titlebar como en el status bar. Como tags, si tu no los agregas en algún lugar, son totalmente inútiles. Para crear un widget usas la función *widget()*:

`mytextbox = widget({ type = "textbox", name = "mytextbox" })`

*mytextbox* ahora contiene un objeto widget. Puedes usar diferentes atributos con diferentes tipos de widgets, seguidos de los grupos de textos mostrados por el textbox.

`mytextbox.text = "Hello, world!`

Creación de la barra de estado (Statusbar)
------------------------------------------

Las barras de estado (Statusbars) son contenedores widgets que puedes colocar en el tope, en la parte inferior o en los bordes derecho o izquierdo del screen.

Primero, crea la barra de estado:

`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`

*mystatusbar* es ahora una variable que contiene un objeto statusbar. Desde que configuramos la posición (*position*) en el tope (*top*), será mostrada en el tope de nuestra pantalla (screen). Obviamente, no lo agregamos al screen, por tanto es inútil por ahora. Hagámoslo:

`mystatusbar.screen = 1`

Nuestra barra de estado es ahora visible en el tope de nuestra pantalla. Bien, la barra de estado no tiene nada, pero es muy útil. ¡Vamos a agregarle widgets!

`-- Create a textbox`
`mytextbox = widget({ type = "textbox", name = "mytextbox" })`
`-- Set text of the textbox`
`mytextbox.text = "Hello, world!"`
`-- Create a statusbar`
`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`
`-- Add widgets to the statusbar`
`mystatusbar:widgets({ mytextbox })`
`-- Add the statusbar on screen #1`
`mystatusbar.screen = 1`

Y voilà. Ahora tenemos una nueva barra de estado en el tope de la pantalla que imprime en pantalla *Hello, world!*. Usando este método, puedes configurar una barra de estado con muchos widgets antes de comenzar a ejecutar awesome, en tu archivo de configuración.

Titlebar creation
-----------------

Like I said before, titlebars are like statusbar except that they are around a window. To create a titlebar, you know the drill:

`mytitlebar = titlebar({ position = "top" })`

Then, you probably want to put widgets in it. It works like statusbars:

`mytitlebartitle = widget({ type = "textbox", name = "mytitlebartitle" })`
`mytitlebar:widgets({ mytitlebartitle })`

A titlebar must be added to a *client object*. We can get the currently focused client: it is stored in *client.focus* variable. Let's do that:

`-- Get the focused client`
`c = client.focus`
`-- Set a titlebar on it`
`c.titlebar = mytitlebar`

Now, the focused client has a nice titlebar on top of it! But it has nothing in it. No, wait! Remember, in the titlebar we already added a *textbox widget*. We can now print the client name in it:

`mytitlebartitle.text = c.name`

The *name* attribute is a string with the client's title, and we set it as the text of the widget. Now the titlebar prints the client's title.

Remember: a titlebar is one object, and a client is another object. So you need to create a titlebar for each client you want a titlebar around.

Using hooks
-----------

With what we seen before, you can build a nice interface. However, imagine you want to update the title printed in the titlebar of a client: you can't. Wait! Here's the solution: hooks.

With hooks, you can define functions that are called when an event happens. Let's try with an example.

When a new client pops up on the screen, awesome runs the function hooked by the *manage* hook. This function is called with the new client as its argument. Here's an example:

`function hook_manage(c)`
`    if c.name:find("mplayer") then`
`        c.floating = true`
`    end`
`end`

First, we defined a *hook_manage* function, with an argument named *c*, which will be the client. Then, we try to find the string *"mplayer"* in the name of the client, and if so, we set it floating.

` awful.hooks.manage.register(hook_manage)`

Then, with that call, we define that the function is to be calledd on each new client arrival is *hook_manage*.

There's a lot of hooks you can use and define: focus, unfocus, manage, unmanage, mouseover, arrange, titleupdate, urgent, timer, … Refer to the manpage for more information.

Executing commands and scripts
------------------------------

You can execute commands and scripts from the lua based configuration. Here is an example function that takes command as parameter and returns its output. This is usefull for example for putting stuff from command outputs to textboxes.

    -- Execute command and return its output. You probably won't only execute commands with one
    -- line of output
    function execute_command(command)
       local fh = io.popen(command)
       local str = ""
       for i in fh:lines() do
          str = str .. i
       end
       io.close(fh)
       return str
    end

Here is an example of putting /proc/loadavg to mytextbox:

     mytextbox.text = " " .. execute_command("cat /proc/loadavg") .. " "

awesome Lua libraries
=====================

awful
-----

[awful](/awful "wikilink") is the default library to interact with awesome, like mouse actions or keybinds.

beautiful
---------

[Beautiful](/Beautiful "wikilink") is as library to theme the look of awesome.

eminent
-------

[Eminent](/Eminent "wikilink") is a Lua library that will enable you to use dynamic tagging. Dynamic tagging in this instance means that the amount of tags you have is not pre-defined, you can give specific tags names, but simply going to the next tag when you're on the last tag will provide you with a brand new one, allowing you to adapt to various usage situations without changing your configuration file to give you more or less tags.

wicked
------

[Wicked](/Wicked "wikilink") is a Lua library for awesome to provide more widgets, like MPD Widgets, CPU usage, memory usage, etc.

[Category: Awesome3](/Category:_Awesome3 "wikilink")