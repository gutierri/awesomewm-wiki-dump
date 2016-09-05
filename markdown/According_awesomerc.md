---
title: According awesomerc
permalink: /According_awesomerc/
---

` screen 0`
` {`
`   general`
`   {`
`     sloppy_focus_raise = false`
`     border = 2`
`     resize_hints = false`
`     snap = 50`
`     new_become_master = true`
`   }`

`   styles`
`   {`
`     #light`
`     focus { font = "terminus 10" fg = "#081408" border = "#444400" bg = "#cccc33" }`
`     normal { font = "terminus 10"  fg = "#111111" border = "#000000" bg = "#ffffcc" }`
`   }`

`   tags`
`   {`
`     tag 1 { layout = "tile" mwfact = 0.68 nmaster = 3 }`
`     tag F { layout = "max" mwfact = 0.68 nmaster = 3 }`
`     tag 3 { layout = "tile" mwfact = 0.68 nmaster = 3 }`
`     tag M { layout = "tile" mwfact = 0.68 nmaster = 3 }`
`     tag 5 { layout = "max" mwfact = 0.68 nmaster = 3 }`
`     tag 6 { layout = "floating" mwfact = 0.68 nmaster = 3 }`
`     tag 7 { layout = "tile" mwfact = 0.68 nmaster = 3 }`
`     tag 8 { layout = "floating" mwfact = 0.68 nmaster = 3 }`
`     tag 9 { layout = "tile" nmaster = 3 }`
`   }`

`   layouts`
`   {`
`     layout tile { image = "/home/calmar/pics/icons/awesome/layouts/tile_grey_small.png" }`
`     layout max { image = "/home/calmar/pics/icons/awesome/layouts/max_grey_small.png" }`
`     layout floating { image = "/home/calmar/pics/icons/awesome/layouts/floating_grey_small.png" }`
`   }`

`   statusbar sbtop`
`   {`
`     position = "top"`
`     height = 22`
`     taglist tl`
`     {`
`       mouse { button = "1" command = "tag_view" }`
`       mouse { button = "1" modkey = {"Mod1"} command = "client_tag" }`
`       mouse { button = "3" command = "tag_toggleview" }`
`       mouse { button = "3" modkey = {"Mod1"} command = "client_toggletag" }`
`       mouse { button = "4" command = "tag_viewnext" }`
`       mouse { button = "5" command = "tag_viewprev" }`
`     }`

`     ######`
`     layoutinfo li`
`     {`
`       mouse { button = "1" command = "tag_setlayout" arg = "+1" }`
`       mouse { button = "4" command = "tag_setlayout" arg = "+1" }`
`       mouse { button = "3" command = "tag_setlayout" arg = "-1" }`
`       mouse { button = "5" command = "tag_setlayout" arg = "-1" }`
`     }`

`     ######`
`     focusicon fi {}`

`     tasklist tasktop`
`     {`
`       show_icons = false`
`       #text_align="left" font = "Sans-7"`
`       mouse { button = "2" command = "client_kill" }`
`       mouse { button = "3" command = "spawn" arg = "9menu_open" }`
`       #mouse { button = "3" command = "client_togglemax" }`
`       mouse { button = "4" command = "client_focusnext" }`
`       mouse { button = "5" command = "client_focusprev" }`
`       mouse { modkey = {"Mod1"} button = "4" command = "client_swapnext" }`
`       mouse { modkey = {"Mod1"} button = "5" command = "client_swapprev" }`
`     }`

`     #textbox tb_cpu { style { fg = "#669966" }  text = " CPU:" }`

`     iconbox ib_cpu { image="/home/calmar/pics/icons/awesome/cpu.png" }`

`     graph gr_cpu`
`     {`
`       #light`
`       data total { scale = false max = 100  draw_style = bottom`
`         vertical_gradient = "true" fg = "#996666" fg_center = "#aa7777" fg_end = "#cc9999" }`
`       data user { scale = false max = 100  draw_style = bottom`
`         vertical_gradient = "true"  fg = "#009900" fg_center = "#00aa00" fg_end = "#00ff00"  }`
`       data nice { scale = false max = 100  draw_style = bottom `
`         vertical_gradient = "true" fg = "#999999" fg_center = "#aaaaaa" fg_end = "#ffffff" }`
`       width = 50`
`       height = "0.80"`
`       #light`
`       bg = "#ffffee"`
`       #dark`
`       #bg = "#000000"`
`       grow = left`
`       bordercolor = "#225522"`
`     }`

`     iconbox ib_mem { image="/home/calmar/pics/icons/awesome/memory.png" }`

`     progressbar pb_mem`
`     {`
`       #light`
`       data mem { bg = "#ffffee" fg = "#6666cc" fg_center = "#9999ee" fg_end = "#ccccff" fg_off = "#ffffff" bordercolor = "#666699" }`
`       data swap { bg = "#ffffee"  fg = "#991111" fg_center = "#cc1111" fg_end = "#ff0000" fg_off = "#ffffff" bordercolor = "#666699" }`
`       width = "32" height = "0.80" `
`       gap = 1`
`       border_padding = 0`
`       border_width = 1`
`       ticks_count = 0`
`       vertical="true"`
`     }`

`     textbox tb_net_in { style {fg = "#009966" } text = " In" }`
`     textbox tb2 { style {fg = "#666666" } text = "/" }`
`     textbox tb_net_out { style {fg = "#996600" } text = "Out" }`

`     emptybox BC { width = 3 }`
`     #iconbox ib_net { image="/home/calmar/pics/icons/awesome/internet.png" }`

`     graph gr_net`
`     {`
`       data in {  vertical_gradient = true scale = true max = 80 fg = "#33cc33" fg_end = "#339933" draw_style = bottom}`
`       data out { vertical_gradient = true scale = true max = 8 fg = "#993300" fg_end = "#cc6600" draw_style = line}`
`       #light`
`       bg = "#ffffee" bordercolor = "#444466"`
`       width = 50 `
`       height = "0.80"`
`       grow = left`
`     }`

`     iconbox ib_df { image="/home/calmar/pics/icons/awesome/diskfree.png" }`

`     emptybox B { width = 3 }`

`     progressbar pb_df_1`
`     {`
`       #light`
`       data root { fg = "#666699" fg_center = "#6666cc" fg_end = "#9999cc" fg_off = "#ffffee" bordercolor = "#4444cc" }`
`       data home { fg = "#669966" fg_center = "#99cc99" fg_end = "#99ff99" fg_off = "#ffffee" bordercolor = "#336633" }`
`       data multi { fg = "#cc6666" fg_center = "#dd9999" fg_end = "#ff9999" fg_off = "#ffffee" bordercolor = "#663333" }`
`       width = "28" height = "0.80" gap = 1`
`       vertical="true"`
`       border_width = 1`
`       border_padding = 0`
`       ticks_gap = 1`
`       ticks_count = 0`
`     }`

`     textbox tb3 { style { fg = "#663333" } text = " [" }`
`     textbox tb_mail { style { fg = "#993333" } }`
`     textbox tb4 { style { fg = "#663333" } text = "] " }`

`     textbox tb_date`
`     {`
`       style { fg = "#009933" } text = " -  "`
`       mouse { button = "5" command = "tag_setlayout" arg = "-1" }`
`       mouse { button = "4" command = "tag_viewprev"}`
`     }`

`   }`
`   statusbar sbbottom`
`   {`
`     position = "bottom"`
`     height = 14`
`     tasklist taskbottom`
`     {`
`       show_icons = true`
`       show = all`
`       styles { normal { font = "Terminus 8" } focus { font = "Terminus 8" }}`
`       mouse { button = "2" command = "client_kill" }`
`       mouse { button = "3" command = "spawn" arg = "9menu_open" }`
`       #mouse { button = "3" command = "client_togglemax" }`
`       mouse { button = "4" command = "client_focusnext" }`
`       mouse { button = "5" command = "client_focusprev" }`
`       mouse { modkey = {"Mod1"} button = "4" command = "client_swapnext" }`
`       mouse { modkey = {"Mod1"} button = "5" command = "client_swapprev" }`
`     }`
`   }`
` }`

` menu >`
` {`
`   styles #light`
`   {`
`     normal { font = "fixed 13" bg = "#cccc00" fg = "#000033" }`
`     focus {  font = "fixed 13"bg = "#ffff00" fg = "#000011" }`
`   }`
`   y = "995"`
` }`

` rules {`
`   rule { name = "MPlayer" float = true }`
`   rule { name = "ding" float = true }`
`   rule { name = "feh" float = true }`
`   rule { name = "firefox" float = true tags = "2" }`
`   rule { name = "fritz" float = true }`
`   rule { name = "gimp" float = true }`
`   rule { name = "gvim" icon = "/home/calmar/pics/icons/awesome/apps/gvim.png" }`
`   rule { name = "urxvt" icon = "/home/calmar/pics/icons/awesome/apps/konsole.png" }`
`   rule { name = "wine" float = true }`
`   rule { name = "xclock" float = true }`
`   rule { name = "xvkbd" float = true }`
` }`

` mouse {`
`     root { button = "3" command = "spawn" arg = "9menu_open" }`
`     client { modkey = {"Mod1"} button = "1" command = "client_movemouse" }`
`     client { modkey = {"Mod1"} button = "2" command = "client_zoom" }`
`     client { modkey = {"Mod1"} button = "3" command = "client_resizemouse" }`
` }`

` keys {`
`   #spawn programs`
`   #--------------`
`   key { modkey = {"Mod1"} key = "Return"                command = "spawn" arg = "exec urxvt 2>/dev/null" }`
`   key { modkey = {"Mod1"} key = "o"                     command = "spawn" arg = "awesome-menu.sh" }`

`   ###############################################################################`
`   #often`
`   #`
`   key { modkey = {"Mod1"} key = "j"                     command = "client_focusnext" }`
`   key { modkey = {"Mod1"} key = "k"                     command = "client_focusprev" }`
`   key { modkey = {"Mod1", "Control"} key = "j"          command = "client_swapnext" }`
`   key { modkey = {"Mod1", "Control"} key = "k"          command = "client_swapprev" }`
`   key { modkey = {"Mod1"} key = "F11"                   command = "client_focusnext" }`
`   key { modkey = {"Mod1"} key = "F10"                   command = "client_focusprev" }`
`   key { modkey = {"Mod1"} key = "c"                     command = "client_kill" }`
`   key { modkey = {"Mod1"} key = "m"                     command = "client_togglemax" }`
`   key { modkey = {"Mod1", "Shift"} key = "m"            command = "client_toggleverticalmax" }`
`   key { modkey = {"Mod1", "Control"} key = "m"          command = "client_togglehorizontalmax" }`
`   key { modkey = {"Mod1"} key = "f"                     command = "client_togglefloating" }`
`   key { modkey = {"Mod1"} key = "s"                     command = "client_togglescratch" }`
`   key { modkey = {"Mod1", "Control"} key = "s"          command = "client_setscratch" }`
`   #view tag ...`
`   key { modkey = {"Mod1"} key = "0"                     command = "tag_view" }`
`   keylist { modkey = {"Mod1"}`
`             keylist = {1, 2, 3, 4, 5, 6, 7, 8, 9}`
`             command = "tag_view"`
`             arglist = {1, 2, 3, 4, 5, 6, 7, 8, 9} }`
`   key { modkey = {"Mod1"} key = "Escape"                command = "tag_prev_selected" }`
`   key { modkey = {"Mod1"} key = "F12"                   command = "tag_viewnext" }`
`   key { modkey = {"Mod1"} key = "F9"                    command = "tag_viewprev" }`
`   key { modkey = {"Mod1"} key = "h"                     command = "tag_viewprev" }`
`   key { modkey = {"Mod1"} key = "l"                     command = "tag_viewnext" }`
`   #`
`   key { modkey = {"Mod1"} key = "comma"                 command = "tag_setmwfact" arg = "-0.05" }`
`   key { modkey = {"Mod1"} key = "period"                command = "tag_setmwfact" arg = "+0.05" }`
`   #toggle layouts`
`   key { modkey = {"Mod1"} key = "space"                 command = "tag_setlayout" arg = "+1" }`
`   key { modkey = {"Mod1", "Control"} key = "space"      command = "tag_setlayout" arg = "-1" }`

`   #moving/resizing client`
`   key { modkey = {"Mod1"} key = "Up"                    command = "client_moveresize" arg = "+0 -18 +0 +0" }`
`   key { modkey = {"Mod1"} key = "Down"                  command = "client_moveresize" arg = "+0 +18 +0 +0" }`
`   key { modkey = {"Mod1"} key = "Left"                  command = "client_moveresize" arg = "-18 +0 +0 +0" }`
`   key { modkey = {"Mod1"} key = "Right"                 command = "client_moveresize" arg = "+18 +0 +0 +0" }`
`   key { modkey = {"Mod1", "Control"} key = "Up"         command = "client_moveresize" arg = "+0 +0 +0 -18" }`
`   key { modkey = {"Mod1", "Control"} key = "Down"       command = "client_moveresize" arg = "+0 +0 +0 +18" }`
`   key { modkey = {"Mod1", "Control"} key = "Right"      command = "client_moveresize" arg = "+0 +0 +18 +0" }`
`   key { modkey = {"Mod1", "Control"} key = "Left"       command = "client_moveresize" arg = "+0 +0 -18 +0" }`
`   # tile hints`
`   key { modkey = {"Mod1"} key = "F6"  command = "client_settilefact" arg = "1.0" }`
`   key { modkey = {"Mod1"} key = "F7"  command = "client_settilefact" arg = "-0.1" }`
`   key { modkey = {"Mod1"} key = "F8"  command = "client_settilefact" arg = "+0.1" }`
`   key { modkey = {"Mod1"} key = "F5"  command = "client_toggletitlebar" }`

`   ###############################################################################`
`   #less often`
`   #`
`   #move clients around: Mod1 + Control + Number`
`   key { modkey = {"Mod1", "Control"} key = "0"          command = "client_tag" }`
`   key { modkey = {"Mod1", "Control"} key = "0"          command = "client_toggletag" }`
`   keylist { modkey = {"Mod1", "Control"}`
`             keylist = {1, 2, 3, 4, 5, 6, 7, 8, 9}`
`             command = "client_tag"`
`             arglist = {1, 2, 3, 4, 5, 6, 7, 8, 9} }`
`   keylist { modkey = {"Mod1", "Control"}`
`             keylist = {1, 2, 3, 4, 5, 6, 7, 8, 9}`
`             command = "client_toggletag"`
`             arglist = {1, 2, 3, 4, 5, 6, 7, 8, 9} }`
`   #layout`
`   key { modkey = {"Mod1", "Control"} key = "h"          command = "tag_setnmaster" arg = "+1" }`
`   key { modkey = {"Mod1", "Control"} key = "l"          command = "tag_setnmaster" arg = "-1" }`
`   #awesome`
`   key { modkey = {"Mod1", "Control"} key = "r"          command = "exec" arg = "/usr/local/bin/awesome" }`
`   key { modkey = {"Mod1", "Control"} key = "q"          command = "quit" }`
`   key { modkey = {"Mod1", "Control"} key = "b"          command = "statusbar_toggle" }`
`   #key { modkey = {"Mod1", "Control"} key = "s"          command = "client_moveresize" }`

`   ###############################################################################`
`   #seldom`
`   #`
`   #additionally view tag `<nr>`: Mod1 + Shift + Number`
`   key { modkey = {"Mod1", "Shift"} key = "0"            command = "tag_toggleview" }`
`   keylist { modkey = {"Mod1", "Shift"}`
`             keylist = {1, 2, 3, 4, 5, 6, 7, 8, 9}`
`             command = "tag_toggleview"`
`             arglist = {1, 2, 3, 4, 5, 6, 7, 8, 9} }`
`   #inc/dec number of columns`
`   key { modkey = {"Mod1", "Shift"} key = "h"            command = "tag_setncol" arg = "-1" }`
`   key { modkey = {"Mod1", "Shift"} key = "l"            command = "tag_setncol" arg = "+1" }`
`   #number of master clients`

`   key { modkey = {"Mod1", "Control"} key = "t"         command = "client_settrans" arg = "+0.1" }`
`   key { modkey = {"Mod1"} key = "t"                    command = "client_settrans" arg = "-0.1" }`
`   #key { modkey = {"Mod1", "Shift"} key = "Return"      command = "client_zoom" arg="-0.10" }`
` }`

[Category:Awesome2](/Category:Awesome2 "wikilink")