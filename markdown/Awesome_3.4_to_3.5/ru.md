---
title: Awesome 3.4 to 3.5 ru
permalink: /Awesome_3.4_to_3.5/ru/
---

Эта страница суммирует изменения между Awesome 3.4 и 3.5. Вы можете использовать данное руководство для обновления своих виджетов и файлов конфигурации. Если вы хотите проверить ваши файлы конфигурации без перезапуска (и возможных поломок) вашей текущей системы, можно использовать [Xephyr](/Using_Xephyr/ru "wikilink")

Новые зависимости Runtime-Only
------------------------------

Awesome 3.5 использует [LGI](https://github.com/pavouk/lgi) для доступа к некоторым библиотекам. Этот пакет называется *Lua GObject Introspection*, он используется для доступа к библиотекам Cairo и Pango.

Однако, сборка awesome не зависит от LGI. Вы можете успешно собрать и установить Awesome, даже если не установлен LGI. Это позволяет пропустить эту зависимость.

Если вы не уверены, что LGI и необходимые файлы у вас есть, вы можете выполнить следующую команду:

    $ lua -e 'lgi = require("lgi") print(lgi.cairo, lgi.Pango, lgi.PangoCairo)'
     table: 0xe74160    table: 0xf40830    table: 0xf168e0


Написанное выше, означает, что все отлично. Однако, если lgi у вас не стоит, вы увидите сообщение подобное описаному ниже:

     $ lua -e 'lgi = require("lgi") print(lgi.cairo, lgi.Pango, lgi.PangoCairo)'
      lua: (command line):1: module 'lgi' not found:
      [здесь будет большой список]

Обратите внимание, что вам требуется LGI 0.6.1 или более новый, потому что предыдущие версии не содержат необходимых зависимостей Cairo. Вышеописанный тест, позволяетизбежать этих проблем.

### Ubuntu 13.04 (raring)

Для выполнения этого теста на Ubuntu, необходимо установить интерпретатор lua:

     $ sudo apt-get install lua5.2

Чтобы не было неразрешенных зависимостей ("LGI" и все необходимые библиотеки "pango" и "cairo") используйте apt, следующая команда установить все необходимое (обратите внимание, что cairo устаноавливается как зависимость другого пакета):

    $ sudo apt-get install lua-lgi gir1.2-pango-1.0

Совместимость Lua 5.2
---------------------

В Lua 5.2 была удалена функция **module()**. Поэтому предется переделывать конфигурационные файлы. Теперь необходимо явно присваивать модули глобальным переменным, при загрузке:

     naughty = require("naughty")

Сигналы
-------

Вам необходимо заменить все вызовы функции **add_signal** на **connect_signal**. Также необходимо заменить **remove_signal** на **disconnect_signal**.

Для замены сигналов и module(), предварительно сделайте резервную копию, можно использовать следующий код:

    export AWCONF=/etc/xdg/awesome/rc.lua; \
    for pattern in 's/add_signal/connect_signal/' 's/remove_signal/disconnect_signal/' \
        's/^require\("(naughty|beautiful|awful)"\)/\1 = \0/'; \
        do sed -r "$pattern" < $AWCONF > $AWCONF.new;done

(Если вы хотите найти лучший способ для модификации файла, попробуйте способ предложенный --[Kardan](/User:Kardan "wikilink"))
Если рекомендация от Kardan верна, то код должен выглядеть так:

    export AWCONF=/etc/xdg/awesome/rc.lua; \
    sed -i.bak -r -e 's/add_signal/connect_signal/' \
                  -e 's/remove_signal/disconnect_signal/' \
                  -e 's/^require\("([^"]*)"\)/\1 = \0/' $AWCONF

Изменение библиотеки "widget"
-----------------------------

### Замена библиотеки "widget"

Теперь больше нет понятия **widget** в API, теперь параметры которые определяют различные виджеты, их **type** могут находиться в разных местах. Один из них новый модуль, называемый **wibox**. Пожалуйста запомните, что wibox больше не часть стандарта API, поэтому используйте *require("wibox")*.

textbox
Заменено на **wibox.widget.textbox()**

imagebox
Заменено на **wibox.widget.imagebox()**

systray
Заменено на **wibox.widget.systray()**

### Textbox Текстовое значение

Теперь вы не можете больше просто присвоить текст свойству **text** текстового виджета. Вместо этого вы должны использовать следующую функцию:

yourbox:set_markup(text)
Set the text content, with support for pango markup.

yourbox:set_text(text)
Set the text content, with no pango markup.

### Imagebox Значение изображения

То же самое относится и к imagebox. Теперь вы можете, непосредственно назначать файл(имя файла) виджету imagebox.

Изменения в библиотеке "awful"
------------------------------

### awful.taglist

Второй аргумент awful.widget.taglist, описывающий тэги(tags), теперь должен быть установлен используя методы фильтрации taglist. В старых конфигурациях использовалось:

        mytaglist[s] = awful.widget.taglist(s, awful.widget.taglist.label.all, mytaglist.buttons)

Теперь так:

           mytaglist[s] = awful.widget.taglist(s, awful.widget.taglist.filter.all, mytaglist.buttons))

### awful.tasklist

Первый аргумент *awful.widget.tasklist* теперь является номером экрана на котором будет отображен tasklist.

Было:

         mytasklist[s] = awful.widget.tasklist(function(c) return awful.widget.tasklist.label.currenttags(c, s) end, mytasklist.buttons)

Новая версия:

        mytasklist[s] = awful.widget.tasklist(s, awful.widget.tasklist.filter.currenttags, mytasklist.buttons)

### awful.menu

**awful.menu** один из модулей который претерпел существенные изменения. Одним из изменений является модификация размера меню. Теперь больше не нужно писать так:

       mymenu = awful.menu({ items = { ..... },
                             width = 300, height = 30 })

Теперь эта запись выглядит так:

       mymenu = awful.menu({ items = { ...... },
                             theme = { width = 300, height = 30 } })

### awful.menu_keys

Добавлено новое ключевое слово **enter** в 'awful.menu_keys. Теперь если вы переопределяете значения по умолчанию 'menu_keys' в вашем 'rc.lua', вам необходимо обновить настройки под новые значения, следующим образом:

     awful.menu.menu_keys = { up    = { "k", "Up" },
                              down  = { "j", "Down" },
                              exec  = { "l", "Return", "Right" },
                             -- the new item
                              enter = { "Right" },
                              --
                              back  = { "h", "Left" },
                              close = { "q", "Escape" },
                            }

Написание своих виджетов
------------------------

Существует отдельное руководство, в котором Вы можете найти, как создавать свои собственные виджеты. Вы можете найти его [здесь](/Writing_own_widgets "wikilink").

Small Prettifications
---------------------

### Градиенты

Вам может показаться, что градиентные виджеты, к примеру progressbars все еще не поддерживаются в 3.5, но... **Теперь они поддерживаются!**

Теперь есть один вид *pattern*, который может устанавливать цвет переднего или заднего фона, или вообще любого цвета, за исключением границы окон. Вы можете ознакоиться с детальной информацией по [gears.color](http://awesome.naquadah.org/doc/api/modules/gears.color.html)

Например, ранее необходимо было писать:

      cpubar:set_gradient_colors({ "#ff0000", "#00ff00", "#0000ff" })

Вместо этого начиная с версии 3.5, необходимо писать так:

      cpubar:set_color({ type = "linear", from = { 0, 0 }, to = { 0, 20 }, stops = { { 0, "#ff0000" }, { 0.5, "#00ff00" }, { 1, "#0000ff" } }})

Дополнительные сведения о линейных(linear) и других видах графиентах в cairo, вы можете найти по ссылке [1](http://kapo-cpp.blogspot.de/2008/01/gradients-in-cairo.html) и [2](http://zetcode.com/gfx/cairo/gradients/).

### Границы виджетов

Действительно, textbox.border_width = 1 или подобного в 3.5 (по состоянию на 10 декабря 2012) нет. [Здесь](http://bpaste.net/show/IIBSq19KrGHwBbGFAUHN/) пользователь [psychon](/User:psychon "wikilink") рассказал о способах реализации данной функции для себя. На [этой странице](/Writing_own_widgets "wikilink") описана функция реализующая эту возможность.

Обновление Вашего rc.lua 3-х сторонним способом
-----------------------------------------------

Если ваш *rc.lua* от 3.4 не очень отличается от стандартного, вы можете использовать трехсторонний метод слияния [kdiff3](http://kdiff3.sourceforge.net/) (Qt) или [meld](http://meldmerge.org/) (GTK):

     $ kdiff3 stock_3.4_rc.lua your_3.4_rc.lua stock_3.5_rc.lua -o your_3.5_rc.lua

    или

    $ meld stock_3.4_rc.lua your_3.4_rc.lua stock_3.5_rc.lua

Они обнаружат изменения внесеные в конфигурацию для 3.4, Вами и разработчиком, и помогут объединить их в новую версию.

[Category:Awesome3.5](/Category:Awesome3.5 "wikilink") [Category:Config](/Category:Config "wikilink")