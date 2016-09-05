---
title: Another pomodoro widget
permalink: /Another_pomodoro_widget/
---

[<File:2012-11-03-222148> 511x63 scrot.png](/File:2012-11-03-222148_511x63_scrot.png "wikilink")

There is a 25+5 seconds [screencast](http://www.youtube.com/watch?v=3YpaDRuyApA&feature=plcp)

` #!/bin/sh`
` # inspired from `[`http://feedelli.org/2012/07/29/bash-command-line-pomodoro-timer.html`](http://feedelli.org/2012/07/29/bash-command-line-pomodoro-timer.html)
` # punch-time-tracking: `[`http://code.google.com/p/punch-time-tracking/`](http://code.google.com/p/punch-time-tracking/)
` #`
` # with awesome-client, you need do something in your rc.lua:`
` #`
` #     ...`
``  #     require("awful.remote") -- make `awesome-client` work ``
` #     ...`
` #     pomodoro = awful.widget.progressbar()`
` #     pomodoro:set_max_value(100)`
` #     pomodoro:set_background_color('#494B4F')`
` #     pomodoro:set_color('#AECF96')`
` #     pomodoro:set_gradient_colors({ '#AECF96', '#88A175', '#FF5656' })`
` #     pomodoro:set_ticks(true)`
` #     ...`
` #     mywibox[s].widgets = {`
` #             ...`
` #             mytaglist[s],`
` #             pomodoro.widget, -- right here`
` #     ...`
` #`
` work=$((25*60))`
` rest=$((5*60))`
` `
` for i in $(seq 100); do`
`     echo "pomodoro:set_value(${i})" | awesome-client`
`     sleep $(echo "scale=3;${work}/100" | bc)`
` done`
` `
` for i in $(seq 100); do`
`     j=$((100-i))`
`     echo "pomodoro:set_value(${j})" | awesome-client`
`     sleep $(echo "scale=3;${rest}/100" | bc)`
` done`