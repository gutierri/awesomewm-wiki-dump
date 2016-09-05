---
title: Awesome 3.4 to 3.5
permalink: /Awesome_3.4_to_3.5/
---

This page summarizes changes which Awesome has undergone between 3.4 and 3.5. You should use it as a guideline for updating your widgets and configuration files.

If you want to test your config without restarting (and potentially breaking) your current running instance of awesome, you can use [Xephyr](/Using_Xephyr "wikilink").

New Runtime-Only Dependency
---------------------------

Awesome 3.5 uses [LGI](https://github.com/pavouk/lgi) for accessing some libraries. This package provides *Lua GObject Introspection*, which is used for accessing the Cairo and Pango libraries.

However, this a not a build dependency, which means that building and installing Awesome may still succeed even if you don't have LGI installed. This makes it easy to miss this dependency.

If you are unsure whether you have LGI and the needed introspection files, you can run the following command:

`$ lua -e 'lgi = require("lgi") print(lgi.cairo, lgi.Pango, lgi.PangoCairo)'`
`table: 0xe74160    table: 0xf40830    table: 0xf168e0`

The above is what this looks like if everything goes well. However, if you don't have lgi, you will see an error message like this:

`$ lua -e 'lgi = require("lgi") print(lgi.cairo, lgi.Pango, lgi.PangoCairo)'`
`lua: (command line):1: module 'lgi' not found:`
`[long error message here]`

Please note that you need LGI 0.6.1 or newer, because older versions don't have the necessary Cairo bindings. The above test should detect whether this is a problem.

### Ubuntu 13.04 (raring)

To perform the above test on Ubuntu, you will need to have the lua interpreter installed:

`$ sudo apt-get install lua5.2`

To pull all the required runtime dependencies (*LGI* and the necessary typelibs for *pango* and *cairo*) using apt, the following command should do the trick (note that the cairo introspection stuff gets pulled as a dependency):

`$ sudo apt-get install lua-lgi gir1.2-pango-1.0`

Lua 5.2 Compatibility
---------------------

In Lua 5.2, the **module()** function was deprecated. This has an effect on user configs. You now have to explicitely assign modules to variables when you load them:

`naughty = require("naughty")`

Signals
-------

You should replace all calls of the **add_signal** function with **connect_signal**. Similarly, **remove_signal** becomes **disconnect_signal**.

To do signal and module() replacements and keep backup of awesome config this snippet can be handy:

    export AWCONF=/etc/xdg/awesome/rc.lua; \
    for pattern in 's/add_signal/connect_signal/' 's/remove_signal/disconnect_signal/' \
    's/^require\("(naughty|beautiful|awful)"\)/\1 = \0/'; \
    do sed -r "$pattern" < $AWCONF > $AWCONF.new;done

(in case there is a better way or place to put this, please go ahead --[Kardan](/User:Kardan "wikilink"))
If Kardan recommendation is true, and generalizing last pattern, the code should be:

    export AWCONF=/etc/xdg/awesome/rc.lua; \
    sed -i.bak -r -e 's/add_signal/connect_signal/' \
                  -e 's/remove_signal/disconnect_signal/' \
                  -e 's/^require\("([^"]*)"\)/\1 = \0/' $AWCONF

"widget" Library Changes
------------------------

### "widget" Library Replacement

There is no longer a thing like **widget** in the API, so different widgets which were provided by its **type** parameter can be found in different places.

One of them is a new module named **wibox**. Please note that wibox is no longer a part of the standard API, so you need to *require("wibox")* to use it.

textbox
Migrated to **wibox.widget.textbox()**

imagebox
Migrated to **wibox.widget.imagebox()**

systray
Migrated to **wibox.widget.systray()**

### Textbox Text Assignment

You should no longer simply assign text to the **text** property of the textbox widget. Instead you should use the following functions:

yourbox:set_markup(text)
Set the text content, with support for pango markup.

yourbox:set_text(text)
Set the text content, with no pango markup.

### Imagebox Image Assignment

Similar things apply to the imagebox. However, you can now also give a file name to the imagebox widget directly.

box:set_image(image)

"awful" Library Changes
-----------------------

### awful.taglist

The second argument to awful.widget.taglist, the tags labels should now be accessed using a widget taglist filter method.

The old default config did this :

`   mytaglist[s] = awful.widget.taglist(s, awful.widget.taglist.label.all, mytaglist.buttons)`

Now :

`   mytaglist[s] = awful.widget.taglist(s, awful.widget.taglist.filter.all, mytaglist.buttons)`

### awful.tasklist

The first argument to *awful.widget.tasklist* is now the screen that the tasklist is for.

The old default config did this:

`    mytasklist[s] = awful.widget.tasklist(function(c) return awful.widget.tasklist.label.currenttags(c, s) end, mytasklist.buttons)`

The new default config instead creates its tasklist like this:

`   mytasklist[s] = awful.widget.tasklist(s, awful.widget.tasklist.filter.currenttags, mytasklist.buttons)`

### awful.menu

**awful.menu** module has undergone a lot of changes. One of them is the modified method of setting the size of the entries. You no longer set it as:

`  mymenu = awful.menu({ items = { ..... },`
`                        width = 300, height = 30 })`

but it is rather like:

`  mymenu = awful.menu({ items = { ...... },`
`                        theme = { width = 300, height = 30 } })`

### awful.menu_keys

A new key `enter` has been added to `awful.menu_keys`. So if you redefine the default `menu_keys` in your `rc.lua`, you will need to update it to include the new entry, like this:

`awful.menu.menu_keys = { up    = { "k", "Up" },`
`                         down  = { "j", "Down" },`
`                         exec  = { "l", "Return", "Right" },`
`                         -- the new item`
`                         enter = { "Right" },`
`                         --`
`                         back  = { "h", "Left" },`
`                         close = { "q", "Escape" },`
`                       }`

Writing Your Own Widgets
------------------------

There is a separate guide which explains how to implement your own widgets. It can be found [here](/Writing_own_widgets "wikilink").

Small Prettifications
---------------------

### Gradients

You may think that widget gradients for, say, progressbars are not supported in 3.5. **False!**

Now it is just one kind of *pattern*, which can be used as a background or foreground colour, or indeed any colour except for window borders. See the [gears.color](http://awesome.naquadah.org/doc/api/modules/gears.color.html) module documentation for details.

For example, where previously you had the following:

` cpubar:set_gradient_colors({ "#ff0000", "#00ff00", "#0000ff" })`

With 3.5 you would do this instead:

` cpubar:set_color({ type = "linear", from = { 0, 0 }, to = { 0, 20 }, stops = { { 0, "#ff0000" }, { 0.5, "#00ff00" }, { 1, "#0000ff" } }})`

For more information about linear (and other) gradients in cairo, you can take a look at [1](http://kapo-cpp.blogspot.de/2008/01/gradients-in-cairo.html) and [2](http://zetcode.com/gfx/cairo/gradients/).

### Widget Borders

Indeed, there's no `textbox.border_width` `=` `1` or anything similar in 3.5 (as of Dec 10th 2012). [Here](http://bpaste.net/show/IIBSq19KrGHwBbGFAUHN/)'s what [psychon](/User:psychon "wikilink") has to say about implementing it yourself; [this page](/Writing_own_widgets "wikilink") describes the functions used there.

Updating Your rc.lua via 3-Way Merge
------------------------------------

If your *rc.lua* for 3.4 isn't terribly different from the default version, you can use a three-way merging tool like [kdiff3](http://kdiff3.sourceforge.net/) (Qt) or [meld](http://meldmerge.org/) (GTK):

`$ kdiff3 stock_3.4_rc.lua your_3.4_rc.lua stock_3.5_rc.lua -o your_3.5_rc.lua`

or

`$ meld stock_3.4_rc.lua your_3.4_rc.lua stock_3.5_rc.lua`

It will detect changes made to the config since 3.4.x, both by you and Awesome developers, and help you merge them into a new version.

[Category:Awesome3.5](/Category:Awesome3.5 "wikilink") [Category:Config](/Category:Config "wikilink")