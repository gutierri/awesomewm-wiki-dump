---
title: Anrxcs Calculator Prompt
permalink: /Anrxcs_Calculator_Prompt/
---

Description
-----------

This is a simple calculator prompt using eval to calculate results.

Using xmessage
--------------

`-- globalkeys = awful.util.table.join(`
`-- ...`
`    awful.key({ modkey            }, "F11", function ()`
`        awful.prompt.run({ prompt = "Calculate: " }, mypromptbox[mouse.screen].widget,`
`            function (expr)`
`                local result = awful.util.eval("return (" .. expr .. ")")`
`                local xmessage = "xmessage -timeout 10 -file -"`
`                awful.util.spawn_with_shell("echo '" .. expr .. ' = ' .. result .. "' | " .. xmessage, false)`
`            end`
`        )`
`    end),`

Using [Naughty](/Naughty "wikilink")
------------------------------------

`require("naughty")`
`-- ...`
`-- globalkeys = awful.util.table.join(`
`-- ...`
`    awful.key({ modkey            }, "F11", function ()`
`        awful.prompt.run({ prompt = "Calculate: " }, mypromptbox[mouse.screen].widget,`
`            function (expr)`
`                local result = awful.util.eval("return (" .. expr .. ")")`
`                naughty.notify({ text = expr .. " = " .. result, timeout = 10 })`
`            end`
`        )`
`    end),`

[Category:Awesome3](/Category:Awesome3 "wikilink")