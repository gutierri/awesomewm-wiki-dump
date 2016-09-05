---
title: Awesome-client
permalink: /Awesome-client/
---

**DRAFT**
---------

### Using *awesome-client* for fun and no profit

On some occasions it may prove useful to control the window manager remotely. For this purpose one can use *awesome-client*. Purpose of this can be to control media player windows or other graphical content with remote control or time how screen looks like based on external triggers.

*awesome-client* sends lua commands to AwesomeWM through dbus.

Short (hopefully growing) list of things that can be done with the client. (done with awesome version 3.4.4)

Commands beginning with "*awesome\#*" are ran inside awesome client and commands beginning with "*$*" are ran as shell commands.

To get "awesome\#" prompt execute 'awesome-client' in your favorite terminal&shell combo.

##### This display current mouse coordinates

*awesome\#* c=mouse.coords()
*awesome\#* return c.x

`  double 1871`

*awesome\#* return c\['x'\]

`  double 1871`

##### This will move the mouse pointer to top left corner

*awesome\#* mouse.coords({x = 10, y = 10})

##### This will change toggle the float status of the active window

*awesome\#* c=awful.client.floating
*awesome\#* c.toggle()

##### This will alter tile proportions if layout supports it

*awesome\#* c=awful.tag
*awesome\#* c.incmwfact(0.1)

##### Notification displaying

Notifications can be used in many ways.

1.  *$* for i in \`seq 1 10\` ; do echo "naughty.notify({ title = 'Title $i', text = 'text $1', timeout = 1 })" | awesome-client ; sleep 2 ; done

<!-- -->

1.  *awesome\#* naughty.notify({ title = "Long Title Name to test limits of notification", text = "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum" })

<!-- -->

1.  That did not quite fit into the box so we try
    *awesome\#* naughty.notify({ title = "Long Title Name to test limits of notification", text = "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum", height = 400, width = 300 })

Have a look at "naughty.lua" for complete list of arguments available for naughty.notify()

##### Write stuff to .xsession-errors file

*awesome\#* io.stderr:write("Lorem ipsum dolor sit amet")

##### listing client windows

Display a popup with list of all client windows
*awesome\#* awful.menu.clients({width=400})

Print to .xsession-errors a list of visible windows
*awesome\#* list=awful.client.visible()
*awesome\#* for k, c in pairs(list) do io.stderr:write(c.name .. "\\n") end

Print to .xsession-errors a list of all windows.
*awesome\#* list=client.get()
*awesome\#* for k, c in pairs(list) do io.stderr:write(c.name .. "\\n") end

Why *awful.client.visible()* and *client.get()* !?!? why not *awful.client.visible()* and *awful.client.get()* OR *client.visible()* and *client.get()* for more consistency??

Former with awful prefix would sound better :)

##### Setting all xterms to float (not ready)

*awesome\#* list=client.get()
*awesome\#* for k, c in pairs(list) do _IF_ c.instance _EQUALS_ "xterm" _THEN_ c.floating.toggle() end

##### Accessing variables and functions (aka 'pay attention when using "local"'

While mostly it seems that sending commands via awesome-client is just like typing lines on the end of your AwesomeWM config and having it auto-restart, there is one aspect in which this is not true: While what you write in rc.lua can access any variable or function declared in the outermost scope, code sent via awesome-client can only access global values (ie. anything you declared 'local' in the rc.lua will be inaccessible -- AwesomeWM will tell you "that variable doesn't exist").

So make sure anything you want to access via awesome-client is not declared local.

##### TODO

(NOTREADY)

? how to replace a function.

(Couldn't you just do function myfunc() ... end?)

? how to view function. To my knowledge this is not possible in Lua. Inspecting/reflecting a list or variable is not possible?

Viewing a function's source is not trivial in stock Lua 5.1; the only way I think you can is to use the debug library to find the source file/string of the function and then use the io library to open that file. This doesn't always work, though. You could probably patch the Lua sources to include a function's source with the function object without much effort.

More effective way of doing this might be with other language, but if one desires to do things from shell scripts then awesome-client is .... awesome.