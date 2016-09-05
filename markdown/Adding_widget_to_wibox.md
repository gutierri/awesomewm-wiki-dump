---
title: Adding widget to wibox
permalink: /Adding_widget_to_wibox/
---

Howto
-----

In order to add a widget (for example named **cpuwidget**) to the main wibox (statusbar, top panel, the thing you see at the top of your screen) do the following steps:

1. Open your **rc.lua** configuration file.

2. Locate the line

`  mywibox[s].widgets = {`

**mywibox\[s\].widgets** is practically a list of widgets for the statusbar on the screen **s**.

-   To add the widget to the left insert it somewhere into the first internal table. The definition should become something like:

`   mywibox[s].widgets = {`
`       {`
`           mylauncher,`
`           mytaglist[s],`
`           cpuwidget, -- This is our custom widget. It will appear after the taglist on the statusbar.`
`           mypromptbox[s],`
`           layout = awful.widget.layout.horizontal.leftright`
`       },`
`       mylayoutbox[s],`
`       mytextclock,`
`       s == 1 and mysystray or nil,`
`       mytasklist[s],`
`       layout = awful.widget.layout.horizontal.rightleft`
`   }`

-   To add the widget to the right insert it somewhere into top-level table. The definition should become something like:

`   mywibox[s].widgets = {`
`       {`
`           mylauncher,`
`           mytaglist[s],`
`           mypromptbox[s],`
`           layout = awful.widget.layout.horizontal.leftright`
`       },`
`       mylayoutbox[s],`
`       mytextclock,`
`       cpuwidget, -- This is our custom widget. It will appear between clock and tray.`
`       s == 1 and mysystray or nil,`
`       mytasklist[s],`
`       layout = awful.widget.layout.horizontal.rightleft`
`   }`

3. Restart Awesome to see the effect.