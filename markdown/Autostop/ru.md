---
title: Autostop ru
permalink: /Autostop/ru/
---

К сожалению не существует никакого hook для выполнения функций lua при выходе; но возможно создать функцию обработчик сигнала 'exit'.

Сигнал выхода вызывается не только при выходе, но также и при перезапуске - поэтому без использвания дополнительной логики использовать этот сигнал безсмыслено. Но для этого нужно экспериментировать.

Обработчик сигнала 'exit' DBus
------------------------------

Добавьте следующие строки в rc.lua

` awesome.add_signal("exit", function() awful.util.spawn("atexit.sh") end)`

Если вы используете Archlinux, вам необходимо использовать следующий код.

` awesome.connect_signal("exit", function () awful.util.spawn("atexit.sh") end)`

Этот код запустит 'atexit.sh'. Конечно, вы можете сделать то же самое используя встроенные функции lua, или запустить другую программу. Используйте свое воображение :-).

Я использую этот вариант для завершения демонов/сервисов, которые я запустил используя dex описанные в [Autostart/ru](/Autostart/ru "wikilink").

Проверено в awesome 3.4.8

Альтернатива: Замена команды Quit
---------------------------------

При эксперементах с решением на DBus описанным выше, я столкнулся с небольшой проблемой. Выход происходил очень быстро, для запуска и выполнения скрипта.

Поэтому я использовал альтернативный метод. Я изменил строки в моем rc.lua с:

`    awful.key({ modkey, "Shift" }, "q" awesome.quit),`

на:

`    awful.key({ modkey, "Shift" }, "q", function awful.util.spawn("konsole -e bb.sh") end),`

Затем в bb.sh, в нем я запускал, то что мне нужно, а затем завершал awesome следующим образом:

`    #!/bin/bash`
`    bleachbit --delete --overwrite adobe_reader.cache adobe_reader.mru adobe_reader.tmp bash.history elinks.history firefox.cache firefox.cookies firefox.dom firefox.download_history firefox.forms firefox.session_restore firefox.site_preferences firefox.url_history firefox.vacuum flash.cache flash.cookies kde.cache kde.recent_documents kde.tmp konqueror.cookies konqueror.current_session konqueror.url_history links2.history nautilus.history realplayer.cookies realplayer.history realplayer.logs skype.chat_logs system.cache system.clipboard system.desktop_entry system.recent_documents system.tmp system.trash thumbnails.cache vim.history`
`    echo ; echo`
`    sudo bleachbit --delete --overwrite apt.autoclean apt.autoremove apt.clean system.localizations system.rotated_logs system.tmp system.trash x11.debug_logs`
`    # exit awesome`
`    echo 'awesome.quit()' | awesome-client`
`    # exit script`
`    exit 0`

Таким образом, все работает, и даже дает время для ввода пароля пользователя sudo.

Еще одним преимуществом является, то, что скрипт не запускается когда я перезагружаюсь, а только когда я завершаю awesome или выхожу из системы.

Проверено в Awesome 3.4.10 на Debian

[Category:Awesome3](/Category:Awesome3 "wikilink")