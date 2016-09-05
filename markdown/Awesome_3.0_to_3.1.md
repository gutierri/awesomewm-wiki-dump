---
title: Awesome 3.0 to 3.1
permalink: /Awesome_3.0_to_3.1/
---

The purpose of this page is to list changes that need to be done in order to use a configuration working with Awesome 3.0 in the new Awesome 3.1 release. This should ease the transition for those not wanting to start over with the default config but rather have quick glance at what sections of their old config to change.

**Feel free to change and move anything you see fit.**

Required changes
----------------

#### statusbar

Statusbar objects no longer exist and you have to be replace this old code

`mystatusbar = statusbar({`
`    position    = "top",`
`    fg          = beautiful.fg_normal,`
`    bg          = beautiful.bg_normal`
`})`

with the following new one

`mystatusbar = wibox({`
`    position    = "top",`
`    fg          = beautiful.fg_normal,`
`    bg          = beautiful.bg_normal`
`})`

Also, the syntax for adding widget changed. Replace

`mystatusbar:widgets({ foo })`

with

`mystatusbar.widgets = { foo }`

#### taglist

In 3.0 the taglist was a capi widget which has now changed since it is now implemented in lua. The new taglist has the following form

`mytaglist = awful.widget.taglist.new(screen, taglabel_function, button_table)`

where you can choose one of

-   awful.widget.taglist.label.noempty
-   awful.widget.taglist.label.all

The button_table looks as follows

`mybuttons = {`
`    button({      }, 1, awful.tag.viewonly),`
`    button({modkey}, 1, awful.client.movetotag),`
`    button({      }, 3, function (tag) tag.selected = not tag.selected end),`
`    button({modkey}, 3, awful.client.toggletag)`
`}`

#### tasklist

The tasklist has gotten the same treatment as the taglist. The new way of creating it is therefore:

`mytasklist = awful.widget.tasklist.new(tasklist_function, button_table)`

Or in an example using one of the predefined tasklist functions

`mybuttons = {`
`   button({}, 1, function (c) client.focus = c; c:raise() end),`
`}`
`mytasklist = awful.widget.tasklist.new(function(c) return awful.widget.tasklist.label.currenttags(c, s) end, config.widgets.tasklists.buttons)`

Optional changes
----------------

#### hooks

In 3.0 the hooks were defined as simple functions and then registered using

`function hook_focus(c)`
`    ...`
`end`
`awful.hooks.focus.register(hook_focus)`

in 3.1 hooks are created the same way keybindings were done

`awful.hooks.focus.register(function (c)`
`    ...`
`end)`

#### squares in taglist

If you use own theme file, you may notice, that there are new *taglist_squares* options:

`taglist_squares_sel = /usr/share/awesome/themes/default/taglist/squarefw.png`
`taglist_squares_unsel = /usr/share/awesome/themes/default/taglist/squarew.png`

#### image widget

Previously displaying an image was done by creating a text widget and setting the text to display the desired image. This is still possible but there is now a special image widget. So instead of

`myimage       = widget({ type = "textbox", align = "right" })`
`myimage.text  = "`<bg image='/path/to/image.png'/>`"`

now it is possible to use

`myimage       = widget({ type = "imagebox", align = "right" })`
`myimage.image = image("/path/to/image.png")`

[Category:Awesome3](/Category:Awesome3 "wikilink") [Category:Config](/Category:Config "wikilink")