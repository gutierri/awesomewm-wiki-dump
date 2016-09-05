---
title: Acc ru
permalink: /Acc/ru/
---

Awesome Configuration Converter
-------------------------------

<b>acc находится в стадии разработки.</b> (во-первых, у него отстойное название и во-вторых, в данный момент ничего не работает).

### Цель проекта

Помочь пользователям перейти с Awesome 2 на Awesome 3, предоставив слой совместимости.

### Как это будет реализовано

Идея состоит в написании библиотеки Lua для интерпретации конфигурационных файлов Awesome 2.X. Файл, использующий acc, будет выглядеть примерно так:

`   require("acc")`
`   acc.translate("/путь/к/файлу/конфигурации/awesome/2.X")`

После этих строк пользователь сможет добавлять код на Lua, постепенно удаляя настройки из файла от версии 2.X. В конечном счете конфигурация будет полностью переписана на Lua и приведённые выше строки можно будет удалить. Звучит неплохо, не так ли? :)

### Это то, что мне нужно! Как бы попробовать этот acc в деле?

Пока никак, он ещё не работает.

### Так... А я могу чем-нибудь помочь?

Вы можете помочь мне его написать. От Вас потребуется хорошее знание настроек Awesome 2.X (и 3.X) и основ Lua (ну их то Вы наверняка знаете, раз смогли настроить Awesome 3 ;)). Вот и всё! Я создал репозиторий mercurial:

-   [<http://hg.kaworu.ch/acc> Репозиторий ACC](/http://hg.kaworu.ch/acc_Репозиторий_ACC "wikilink")

Вы также можете помочь, отправив мне свой конфигурационный файл Awesome 2.X и приняв участие в тестировании acc, когда он заработает. Это сильно ускорит процесс разработки.

### Разработка

Я думаю, что процесс нужно разбить на 4 шага:

-   лексический анализатор
-   парсер
-   анализатор
-   интерпретатор

The lexer/parser are used to parse the libconfuse config file in Lua. The analyzer ensure that the config file looks like an awesome config (for example, should reject "keys { screen 2 { ... } }") The interpreter setup a default config and then translate the AST into awesome's Lua calls.

-   The Lexer seems ok, and I'm currently working on the parser.
-   update at 1231124037 time_t : parser seems ok, will begin the analyzer soon

[Category:Awesome2](/Category:Awesome2 "wikilink") [Category:Awesome3](/Category:Awesome3 "wikilink")