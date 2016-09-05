---
title: Add Persian keyboard keyoaut
permalink: /Add_Persian_keyboard_keyoaut/
---

However [Changing Keyboard map](https://awesome.naquadah.org/wiki/Change_keyboard_maps) explains how to in details, But I decide to wrote a function to change my layout to [Persian Language](http://en.wikipedia.org/wiki/Persian_language) map, You can add the following code to rc.lua for using it:

` -- the following code has been tested on 3.5 `
` keyboard_layout = {"us","ir"}`
` current_layout =  keyboard_layout[1]`
` switch = function() `
`   if current_layout == "us" then current_layout = keyboard_layout[2]`
`   else current_layout =  keyboard_layout[1] end`
`   os.execute("setxkbmap " .. current_layout)`
`   naughty.notify{text="Keyboard layout has been changed to " .. current_layout}`
` end  `
` globalkeys = awful.util.table.join(globalkeys, awful.key({ "Mod1"  }, "Shift_L", function()  switch() end ))`

You can change between English and Persian language with ALT_L + SHIFT_L keys.