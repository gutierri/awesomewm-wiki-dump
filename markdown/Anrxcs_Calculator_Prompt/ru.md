---
title: Anrxcs Calculator Prompt ru
permalink: /Anrxcs_Calculator_Prompt/ru/
---

Описание
--------

Это простой калькулятор для строки prompt использующий eval для расчета результата.

Использование xmessage
----------------------

Добавльте следующий код в секцию globalkeys в rc.lua

`    awful.key({ modkey            }, "F11", function ()`
`        awful.prompt.run({ prompt = "Calculate: " }, mypromptbox[mouse.screen].widget,`
`            function (expr)`
`                local result = awful.util.eval("return (" .. expr .. ")")`
`                local xmessage = "xmessage -timeout 10 -file -"`
`                awful.util.spawn_with_shell("echo '" .. expr .. ' = ' .. result .. "' | " .. xmessage, false)`
`            end`
`        )`
`    end),`

Теперь при нажатии Mod+F11 в строке promptbox вы вводите выражение, и после нажатия Enter вам выводится результат.

Использование [Naughty](/Naughty/ru "wikilink")
-----------------------------------------------

В начало файла rc.lua добавьте:

`require("naughty")`

Добавльте следующий код в секцию globalkeys в rc.lua

`    awful.key({ modkey            }, "F11", function ()`
`        awful.prompt.run({ prompt = "Calculate: " }, mypromptbox[mouse.screen].widget,`
`            function (expr)`
`                local result = awful.util.eval("return (" .. expr .. ")")`
`                naughty.notify({ text = expr .. " = " .. result, timeout = 10 })`
`            end`
`        )`
`    end),`

[Category:Awesome3](/Category:Awesome3 "wikilink")