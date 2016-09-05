---
title: Active RAM
permalink: /Active_RAM/
---

IceBrain's Active RAM Widget
============================

This function returns the Active memory usage, according to /proc/meminfo. It is a "light" function as it doesn't spawn any processes and it's composed of plain Lua, so it should work on all Awesome versions.

Function code
-------------

Paste this inside your rc.lua:

There are two versions, copy only one, according to your taste. The first prints the usage in MB:

` function activeram()`
`     local active`
`     for line in io.lines('/proc/meminfo') do`
`         for key, value in string.gmatch(line, "(%w+):\ +(%d+).+") do`
`             if key == "Active" then active = tonumber(value) end`
`         end`
`     end`
`      `
`     return string.format("%.2fMB",(active/1024))`
` end`

This prints in percentage of total RAM used:

` function activeram()`
`   local active, total`
`   for line in io.lines('/proc/meminfo') do`
`       for key, value in string.gmatch(line, "(%w+):\ +(%d+).+") do`
`           if key == "Active" then active = tonumber(value)`
`           elseif key == "MemTotal" then total = tonumber(value) end`
`       end`
`   end`
`   `
`   return string.format("%.0f%%",(active/total)*100)`
` end`

Usage
-----

Now, you can use a normal textbox to show you RAM usage:

First, create the widget:

` meminfo = widget({ type = "textbox", align = "right" })`

Then, assign a hook to update it:

` awful.hooks.timer.register(10, function() meminfo.text = activeram() end)`

Finally, assign it to your wibox:

` mywibox[s].widgets = {   ...`
`                          meminfo,`
`                          ...`
`                          s == 1 and mysystray or nil }`

Update for version 3.5.5
------------------------

1. Create a file called **activeram.lua** and paste one of the following two versions:

1a. The first version (in MB)

`   local wibox = require("wibox")`
`   local awful = require("awful")`
`   `
`   activeram_widget = wibox.widget.textbox()`
`   activeram_widget:set_align("right")`
`   `
`   function update_activeram(widget)`
`       local active`
`       for line in io.lines('/proc/meminfo') do`
`           for key, value in string.gmatch(line, "(%w+):\ +(%d+).+") do`
`               if key == "Active" then active = tonumber(value) end`
`           end`
`       end`
`   `
`       widget:set_markup(string.format("%.2fMB",(active/1024)))`
`   end`
`   `
`   update_activeram(activeram_widget)`
`   `
`   memtimer = timer({ timeout = 10 })`
`   memtimer:connect_signal("timeout", function () update_activeram(activeram_widget) end)`
`   memtimer:start()`

1b. The second version (in percentage)

`   local wibox = require("wibox")`
`   local awful = require("awful")`
`   `
`   activeram_widget = wibox.widget.textbox()`
`   activeram_widget:set_align("right")`
`   `
`   function update_activeram(widget)`
`       local active, total`
`   for line in io.lines('/proc/meminfo') do`
`       for key, value in string.gmatch(line, "(%w+):\ +(%d+).+") do`
`           if key == "Active" then active = tonumber(value)`
`           elseif key == "MemTotal" then total = tonumber(value) end`
`       end`
`   end`
`   `
`       widget:set_markup(string.format("%.2fMB",(active/1024)))`
`   end`
`   `
`   update_activeram(activeram_widget)`
`   `
`   memtimer = timer({ timeout = 10 })`
`   memtimer:connect_signal("timeout", function () update_activeram(activeram_widget) end)`
`   memtimer:start()`

2. Paste the below line in **rc.lua** near the top where other require lines are:

`   require("activeram")`

3. Paste the below line in **rc.lua** as well, except it should be just before the line with **"right_layout:add(mylayoutbox\[s\])"**:

`   right_layout:add(activeram_widget)`

[Category:Awesome3](/Category:Awesome3 "wikilink")