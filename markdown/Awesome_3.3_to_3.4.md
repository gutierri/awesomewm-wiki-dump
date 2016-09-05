---
title: Awesome 3.3 to 3.4
permalink: /Awesome_3.3_to_3.4/
---

### Window management layouts

Most noticeable change is that *floating* layout is now used on all tags by default. You can change the default for all tags with a simple modification of *rc.lua*. Tag section is near the top of the default *rc.lua* and tags are setup like this:

` -- Each screen has its own tag table.`
` tags[s] = awful.tag({ 1, 2, 3, 4, 5, 6, 7, 8, 9 }, s)`

To have the *tile* layout on all tags, you can change that to:

` -- Each screen has its own tag table.`
` tags[s] = awful.tag({ 1, 2, 3, 4, 5, 6, 7, 8, 9 }, s, awful.layout.suit.tile)`

In awesome **3.4.1** layout argument can also be a table, and the following example also shows how to name your tags:

` -- Each screen has its own tag table.`
` tags[s] = awful.tag({ "one", "www", "irc", 4, 5, 6, 7, 8, 9}, s,`
` { layouts[2], layouts[1], layouts[1],          -- Tags: 1, 2, 3`
`   layouts[5], layouts[6], layouts[2],          --       4, 5 ,6`
`   layouts[2], layouts[1], layouts[3]           --       7, 8, 9`
` })`

In awesome **3.4.3** layout argument is set to *layouts\[1\]* by default, and the first layout in the layouts table is floating. Changing the default layout is a matter of reordering your layouts table.

If you are wondering why floating is the default layout, know that awesome is trying to go past the "tiling window manager" label. Maybe it's not in the same category as *openbox* but it also doesn't fit in the same drawer as *wmii* or *subtle*. It is probably closest to *fvwm*, at least by philosophy of being a frame-work window manager. If you are not convinced it's best to read what the lead developer has to say about it: [Taking the other direction](http://julien.danjou.info/blog/2009/taking-the-other-direction).

### Widget Layouts

One of the bigger changes in awesome from version 3.3 to 3.4 is the introduction of widget layouts. These allow controlling the placement of widgets from Lua, to a much bigger degree than with the "old" .align property on widgets.

#### How do they work?

Each widget table has a .layout field. This field points to a function which takes a table containing widgets and the area these widgets should be placed in as its parameters and returns a table containing geometries (i.e. width, height, x and y position) for all widgets. That function is simply called with the widget table as its argument. Sounds simple, right? Well, it is :)

#### How do I use them?

First, remove all .align properties from your widgets, they are no longer effective. Then, put widgets which had the same .align into tables and set the tables .layout field to the right layout function (these are described below). Put these tables into the widgets table of your wibox and set its layout field accordingly.

Make sure that your tasklist widget is the last one in the widgets table, because it uses the flex layout by default.

#### Which layouts are available?

At the moment, the following widget layouts are available:

awful.widget.layout.horizontal.leftright:
places all widgets it contains left to right onto the wibox. Similar to the old .align = "left"

<!-- -->

awful.widget.layout.horizontal.rightleft:
like the previous, only places the widgets right to left. Similar to .align = "right", although the order of widgets is inverted, i.e. the widget which appears first in a table with .layout = awful.widget.layout.horizontal.rightleft is placed rightmost

<!-- -->

awful.widget.layout.horizontal.flex:
similar to the old .align = "flex", but it needs to be placed *last* in the widget table, as it uses all horizontal space it has available at its point of execution.

<!-- -->

awful.widget.layout.vertical.flex:
similar to the horizontal flex layout, this layout places widgets on top of each other, which for example allows stacking an imagebox and a textbox on top of each other to form an icon with caption.

### Widgets

Progressbar and graph widgets are now implemented in Lua, and C widgets are deprecated (to be removed in the next awesome release). You can create progressbars and graphs by calling *awful.widget.progressbar* and *awful.widget.graph*.

Using the old C API

`   mygraphwidget = widget({ type = "graph", name = "mygraphwidget", align = "right" })`
`   mypbarwidget  = widget({ type = "progressbar", name = "mypbarwidget", align = "right" })`

Using awful.widget

`   mygraphwidget = awful.widget.graph()`
`   mypbarwidget  = awful.widget.progressbar()`

When putting the widget in your wibox, make sure to access the actual widget, which is stored in the .widget field:

`   mytextclock,`
`   mygraphwidget.widget,`
`   mypbarwidget.widget,`

Check the respective API docs for the [graph](http://awesome.naquadah.org/doc/api/modules/awful.widget.graph.html) and [progressbar](http://awesome.naquadah.org/doc/api/modules/awful.widget.progressbar.html) widgets to learn the new settings.

### Invaders

After the *Awesome User Survey* was completed it was decided that *Invaders* will not be a part of the standard awesome distribution.

### Timers

Awesome 3.4 has a new Lua [timer](http://awesome.naquadah.org/doc/api/modules/timer.html) object. This new timer API is the preferred way to schedule periodic events; the old awful.hooks.timer has been deprecated.

Update your old code from

`-- Hook called every 30s`
`awful.hooks.timer.register(30, function() mytextbox.text = foo() end)`

to

`mytimer = timer({ timeout = 30 })`
`mytimer:add_signal("timeout", function() mytextbox.text = foo() end)`
`mytimer:start()`

### D-Bus

The dbus name has changed from *org.awesome.* to **org.naquadah.awesome**

### Signals

Awesome 3.4 introduces a new way to manage events. It replaces the old hook system and some other functions like widget.mouse_enter

Instead of:

` mytextbox.mouse_leave = function ()`
`     --CODE`
` end`

Use:

` mytextbox:add_signal("mouse::leave", function ()`
`     --CODE`
` end)`

### Buttons

Syntax went through little changes.

Old:

` mytextbox:buttons({`
`   button({ }, 1, function()`
`       --CODE`
`   end`
` })`

New:

` mytextbox:buttons(awful.util.table.join(`
`   awful.button({ }, 1, function()`
`       --CODE`
`   end)`
` ))`

### Client rules

Application specific behaviour (previously defined in two tables; *floatapps* and *apptags*) was replaced by the **awful.rules** module. All rules are now defined in the *awful.rules.rules* table, and syntax is documented [here](http://awesome.naquadah.org/doc/api/modules/awful.rules.html#rules).

### Themes

**Zenburn** theme is now a part of the standard awesome distribution, in addition to the old themes; *default* and *sky*.

**\*Bug in 3.4.11\***: If your theme.lua was broken upon upgrading to 3.4.11, it could be because *wallpaper_cmd* requires a table. The bug is already fixed in awesome-git. Meanwhile you can get your theme working again by using the line:

` theme.wallpaper_cmd = { 42 }`

[Category:Awesome3.4](/Category:Awesome3.4 "wikilink") [Category:Config](/Category:Config "wikilink")