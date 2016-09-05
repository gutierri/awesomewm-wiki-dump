---
title: Anrxcs WebSearch Prompt ru
permalink: /Anrxcs_WebSearch_Prompt/ru/
---

Описание
--------

Это приглашение для веб-поиска использующее YubNub.org. YubNub позволяет вам передавать ваш поисковый запрос практически в любую поисковую систему, таким образом предоставляя вам доступ ко всему интернету всего лишь из одной строки. Давайте разберем как это работает:

-   Поисковый запрос: <b>awesome window manager</b>
    -   будет производить поиск на google.com строку "awesome window manager" поскольку google является поисковиком по умолчанию
    -   Поисковый запрос: <b>g awesome window manager</b>
        -   будет делать то же самое, т.к. 'g' это команда для поиска в google
-   Поисковый запрос: <b>gim awesome window manager</b>
    -   будет искать изображения по запросу "awesome window manager"
-   Поисковый запрос: <b>apr awesome</b>
    -   Будет искать репозитории Arch Linux содержащие "awesome"
-   Поисковый запрос: <b>aur awesome</b>
    -   будет искать пользовательские репозитории Arch Linux содержащие 'awesome'
-   Поисковый запрос: <b>wp awesome window manager</b>
    -   будет искать в Wikipedia строку "awesome window manager"
-   ...и так далее.

Уже доступны тысячи команд, для изучения их, или даже для определения собственных посетите <http://yubnub.org>
Еще раз, для понимания, вам нет необходимости вносить дополнительные данные или команды в rc.lua, YubNub берез всю заботу об этом на себя, все что вам нужно, это составить корректный поисковый запрос.

Код приведенный ниже осуществляет поиск в YubNub через Firefox, а затем автоматически переключаясь на тег присвоенный Firefox - в данном коде не производится поиска тега, т.к. зачастую он уже жестко прописан, в нашем примере это тег <b>3</b>.

Клавиатурное сочетание и код
----------------------------

    -- Prompt menus
    -- ...
    -- ...
    -- ...
        awful.key({ altkey }, "F12", function ()
            awful.prompt.run({ prompt = "Web search: " }, mypromptbox[mouse.screen].widget,
                function (command)
                    awful.util.spawn("firefox 'http://yubnub.org/parser/parse?command="..command.."'", false)
                    -- Switch to the web tag, where Firefox is, in this case tag 3
                    if tags[mouse.screen][3] then awful.tag.viewonly(tags[mouse.screen][3]) end
                end)
        end),

[Category:Awesome3](/Category:Awesome3 "wikilink")