---
title: Awesome 3.0 to 3.1 ru
permalink: /Awesome_3.0_to_3.1/ru/
---

Цель этой страницы - предоставить список изменений, которые необходимо будет сделать, чтобы файл конфигурации Awesome 3.0 заработал в Awesome 3.1. Это должно облегчить переход для тех пользователей, которые предпочитают модифицировать свой файл конфигурации вместо использования стандартного.

Обязательные изменения
----------------------

#### statusbar

Объекты типа statusbar больше не существуют, поэтому старый код

`mystatusbar = statusbar({`
`    position    = "top",`
`    fg          = beautiful.fg_normal,`
`    bg          = beautiful.bg_normal`
`})`

нужно заменить новым

`mystatusbar = wibox({`
`    position    = "top",`
`    fg          = beautiful.fg_normal,`
`    bg          = beautiful.bg_normal`
`})`

Также изменился синтаксис для добавления виджета. Замените

`mystatusbar:widgets({ foo })`

на

`mystatusbar.widgets = { foo }`

#### taglist

В версии 3.1 виджет taglist переписан на Lua. Новый виджет создаётся в следующей форме

`mytaglist = awful.widget.taglist.new(screen, taglabel_function, button_table)`

можно выбирать один из

-   awful.widget.taglist.label.noempty
-   awful.widget.taglist.label.all

Таблица button_table имеет вид

`mybuttons = {`
`    button({      }, 1, awful.tag.viewonly),`
`    button({modkey}, 1, awful.client.movetotag),`
`    button({      }, 3, function (tag) tag.selected = not tag.selected end),`
`    button({modkey}, 3, awful.client.toggletag)`
`}`

#### tasklist

Виджет tasklist подвергся тем же изменениям, что и taglist. Поэтому новый способ его создания:

`mytasklist = awful.widget.tasklist.new(tasklist_function, button_table)`

Или можно использовать одну из предопределённых функций tasklist

`mybuttons = {`
`   button({}, 1, function (c) client.focus = c; c:raise() end),`
`}`
`mytasklist = awful.widget.tasklist.new(function(c) `
`                                         return awful.widget.tasklist.label.currenttags(c, s) `
`                                       end, `
`                                       config.widgets.tasklists.buttons)`

Необязательные изменения
------------------------

#### hooks (ловушки)

В версии 3.0 ловушки определялись как обычные функции и затем регистрировались с помощью функции

`function hook_focus(c)`
`    ...`
`end`
`awful.hooks.focus.register(hook_focus)`

В версии 3.1 ловушки создаются так же, как привязки клавиш

`awful.hooks.focus.register(function (c)`
`    ...`