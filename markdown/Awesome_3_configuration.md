---
title: Awesome 3 configuration
permalink: /Awesome_3_configuration/
---

Background
----------

**awesome 3** now uses a configuration file based on the [Lua](http://www.lua.org) language. Most people coming from the awesome 2 world will cry seeing that the old configuration file in libconfuse format has disappeared, others will hate this because they never understood the [ion](http://modeemi.fi/~tuomov/ion/) configuration file, but neither did I.

Why Lua? Because it offers more control. It offers conditional behavior: you wanted awesome to do something when something happens, you were not able to. Now, you are. You're able to do almost **anything** (in the range of what a window manager can do).

Files
-----

-   ~/.config/awesome/rc.lua
-   /etc/xdg/awesome/rc.lua

If awesome cannot find ~/.config/awesome/rc.lua, or fails to load it, it falls back to using /etc/xdg/awesome/rc.lua.

Lua basics
----------

[Lua](http://www.lua.org/) is a **simple** language. While an intimate understanding of it is not an absolute prerequisite to use awesome, one's ability does directly correlate with the capacity to customize and fully realize awesome's potential.

While awesome 3 was originally conceptualized and developed for power users with some computer science background, many current users only learned Lua for and by using awesome. In fact, many current awesome users are not programmers, developers, or "power user" types - simply people who wanted more from their window manager.

If you're not familiar at all with computer languages, i.e. if you do not know what *objects*, *methods* and *arguments* are, well, this particular document may be a bit beyond you. You can learn some basics and come back to this, or press on and start your learning here. No matter from where you are starting, there is enough help to get even the most novice user up and running with awesome. So, if you're really motivated, you can learn enough basics to configure and control awesome.

The best way to learn Lua is to spend one or two hours reading the [Programming In Lua](http://www.lua.org/pil/) book, which is nice and will give you an overview of what's doable in Lua. Do not hesitate to bookmark important pages talking about flow control, useful statements, etc. That will help: it's awful to look for the *for* syntax when trying to type lines of code.

To get a quick and precise glimpse at Lua language please consider reading [this](/The_briefest_introduction_to_Lua "wikilink") article. It will provide you with a basic knowledge enough to start configuring your Awesome.

awesome object types
--------------------

For people coming from outer space who did not use awesome, here are the basic objects awesome will give you and you will have to manipulate soon.

<span id="screen">Screen</span>
There is no real screen object in awesome, but we will talk about screens later. A screen is a physical monitor plugged into your computer. They are represented by their index, starting at 1.

<span id="client">Client</span>
A client is a window. I guess I'm clear.

<span id="tag">Tag</span>
A tag is something like a workspace/desktop but the concept is less rigid.

Each client has at least one tag assigned to it. Each screen has at least one tag.

At any time, on a screen, you can view any tag number. Watching no tag will hide every client. Watching one or more tags will show you all clients which have these tags.

<span id="widget">Widget</span>
Widgets are objects which draw things on the screen. There are several types of widget. For example there's a text box widget which prints text on the screen. There's also a icon box widget which draws icons on the screen.

<span id="titlebar">Titlebar</span>
A titlebar is a bar which is around a client and attached to it. You can put widgets in it.

<span id="statusbar">Statusbar</span>
A statusbar is a bar which is fixed at the edge of a screen. You can put widgets in it.

Building our interface
----------------------

To build our interface, we need to use awesome functions and methods: **everything** is documented in the [Lua API documentation](http://awesome.naquadah.org/doc/api/) and in the awesomerc(5) manpage. Read them. They are your development reference. I repeat: read it, everything is documented in this manpage and in luadoc.

Which manpage?… awesomerc(5). Good. You're following.

### Tag creation

If you start awesome without any Lua operation, you will have 0 tags, which is fatal. awesome wants at least one tag.

So first, we need to create a tag. How do we do that? We look for tag() and its documentation, the tag creator function in the manpage. It says that the parameter of the function must be a table with at least a name attribute. Let's do that.

`mytagone = tag({ name = "one" })`

Now you declared an object *mytagone* to be a tag object, and its name is *one*.

`mytagtwo = tag({name = "two"})`

Now, we have a second tag object "mytagtwo": its name is *two*, and by default it arranges clients with the floating algorithm, which, like its name suggests, lets the clients float.

Creating objects is nice, but well, for now it's useless. Remember: each screen has at least one tag. So we need to add these created tags to our first screen. The *screen* attribute of the tag object does that for us, we only need to give it the screen number value.

`mytagone.screen = 1`

We just added that tag to the screen \#1. If we have a second screen (multi-head: Zaphod, Xinerama or XRandR are identical for awesome), we can add our second tag to this screen:

`mytagtwo.screen = 2`

And if we have 3 screens, we need to... Well, I guess you got it.

If you want to know how many screens you have, you can use the *screen.count()* function.

You can manipulate tag viewing with the *selected* attribute. If you want to view your tag *one*:

`mytagone.selected = true`

If you want to hide it, you can obviously set the value to *false*.

You can manipulate all the tag settings with its attributes. See the documentation.

### Client manipulation

Sometimes, you will get client objects. This is very useful in hook functions, which we will discuss later.

A client is an object which has methods too, like tags and others. We decide that the *c* variable is our client object. For example, if we want to set a client floating we can use the *floating* attribute like this:

`c.floating = true`

Some methods return values that can be used later for analysis. If we want to know if a client is floating, we can get the *floating* attribute:

`is_client_floating = c.floating`

The variable *is_client_floating* now contains a boolean value: *true* if the client is floating or *false* if the client is not.

We can combine these 2 functions to create a toggle function: this function will set the client floating state to true if it's not, or to false if it is.

`function client_floating_toggle(c)`
`    if c then`
`        c.floating = not c.floating`
`    end`
`end`

We can also use that to toggle a tag on our client:

`function client_tag_with_tag_one(c)`
`    if c then`
`        local t = c:tags()`
`        table.insert(t, one)`
`        c:tags(t)`
`    end`
`end`

### Widget creation

Widgets are small objects that can be placed either on a titlebar or on a statusbar. Like tags, if you do not add them somewhere, they're totally useless. To create a widget you use the *widget()* function:

`mytextbox = widget({ type = "textbox" })`

*mytextbox* now contains a widget object. You can use different attributes with different widget types, the following sets the text displayed by the textbox.

`mytextbox.text = "Hello, world!`

### Statusbar creation

Statusbars are widget containers (wiboxes) that you can put on the top, bottom, left or right screen edges.

First, create a wibox:

`mystatusbar = wibox({ position = "top" })`

*mystatusbar* is now a variable which contains a wibox object. Since we set *position* to *top*, it will be placed on top of our screen. Obviously, we did not add it to the screen, so it's useless for now. Let's do it:

`mystatusbar.screen = 1`

Our statusbar is now visible on top of the screen. Well, it does nothing, so it's not very useful. Let's add some widgets!

`-- Create a textbox`
`mytextbox = widget({ type = "textbox", name = "mytextbox" })`
`-- Set text of the textbox`
`mytextbox.text = "Hello, world!"`
`-- Create a statusbar`
`mystatusbar = awful.wibox({ position = "top", name = "mystatusbar" })`
`-- Add widgets to the statusbar`
`mystatusbar.widgets = { mytextbox }`
`-- Add the statusbar on screen #1`
`mystatusbar.screen = 1`

And voilà. We now have a brand new statusbar on top of the screen which prints *Hello, world!*. Using this method, you can set up a statusbar with lots of widgets before starting awesome, in your configuration file.

### Titlebar creation

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

### Using hooks/signals

hooks have been replaced by [Signals](/Signals "wikilink") in 3.4+

With what we have seen above, you can build a nice interface. However, imagine you want to update the title printed in the titlebar of a client: you can't. Wait! Here's the solution: hooks.

With hooks, you can define functions that are called when an event happens. Let's try with an example.

When a new client pops up on the screen, awesome runs the function hooked by the *manage* hook. This function is called with the new client as its argument. Here's an example:

`function hook_manage(c)`
`    if c.name:find("mplayer") then`
`        c.floating = true`
`    end`
`end`

First, we defined a *hook_manage* function, with an argument named *c*, which will be the client. Then, we try to find the string *"mplayer"* in the name of the client, and if so, we set it floating.

` awful.hooks.manage.register(hook_manage)`

This defines that the function to be called on each new client arrival is *hook_manage*.

There's a lot of hooks you can use and define: focus, unfocus, manage, unmanage, mouseover, arrange, titleupdate, urgent, timer, … Refer to the manpage for more information.

### Executing commands and scripts

You can execute commands and scripts from the lua based configuration. Here is an example function that takes command as parameter and returns its output. This is useful for example for putting stuff from command outputs to textboxes.

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
---------------------

[Awful](/Awful "wikilink")
The default library to interact with awesome, like mouse actions or keybinds.

[Beautiful](/Beautiful "wikilink")
The library to theme the look of awesome.

[Eminent](/Eminent "wikilink")
A Lua library that will enable you to use dynamic tagging. Dynamic tagging in this instance means that the amount of tags you have is not pre-defined, you can give specific tags names, but simply going to the next tag when you're on the last tag will provide you with a brand new one, allowing you to adapt to various usage situations without changing your configuration file to give you more or less tags.

[Shifty](/Shifty "wikilink")
An up-to-date dynamic tagging library + client matching

[Naughty](/Naughty "wikilink")
A notification library.

[Wicked](/Wicked "wikilink")
A Lua library for awesome to provide more widgets (e.g. MPD Widgets, CPU usage, memory usage, etc.).

[Category: Awesome3](/Category:_Awesome3 "wikilink") [Category:Config](/Category:Config "wikilink")