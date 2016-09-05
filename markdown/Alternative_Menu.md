---
title: Alternative Menu
permalink: /Alternative_Menu/
---

Batz Alternative Menu
---------------------

### Introduction

I really liked the built-in menu for selecting a particular window quickly. However, the up/down mechanism was too limiting (not efficient). I modified the menu.lua code to add a hint to quickly raise a window.

### Diff from awesome 3.4.6 menu.lua

`32c32`
`< module("awful.menu")`
`---`
`> module("batz.menu")`
`34a35,36`
`> --keys = "aoeuidhtns',.pyfgcrl;qjkxbmwvz1234567890"`
`> keys = "asdfghjkl;qwertyuiopzxcvbnm,./1234567890"`
`189c191,197`
`<         check_access_key(cur_menu, key)`
`---`
`>         local i = keys:find(key)`
`>         if not(i == nil) and i <= #cur_menu.items then`
`>             item_enter(cur_menu, i)`
`>             exec(cur_menu, i)`
`>         else`
`>             check_access_key(cur_menu, key)`
`>         end`
`278a287`
`>     local keys_index = 1`
`280c289`
`<         cls_t[#cls_t + 1] = { util.escape(c.name) or "",`
`---`
`>         cls_t[#cls_t + 1] = { keys:sub(keys_index, keys_index) .. " " .. util.escape(c.name) or "",`
`288a298`
`>         keys_index = keys_index + 1`

### Usage

-   Place in ~/.config/awesome/
-   Add the following to the imports of rc.lua

` require("menu")`

-   To change the hint keys add the following to rc.lua (e.g. a dvorak layout)

` batz.menu.keys = "aoeuidhtns',.pyfgcrl;qjkxbmwvz123456789"`

-   Usage is just like awesome.menu (in rc.lua)

` awful.key({ altkey,           }, "Escape",`
`   function ()`
`     batz.menu.menu_keys.down = { "j", "Down" }`
`     batz.menu.menu_keys.up = { "k", "Up" }`
`     local cmenu = batz.menu.clients({width=245}, { keygrabber=true, coords={x=525, y=330} })`
`   end)`

-   Now when M-Esc is pressed the menu is show, and pressing the key associated with the item raises it.
