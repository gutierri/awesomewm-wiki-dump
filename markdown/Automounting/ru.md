---
title: Automounting ru
permalink: /Automounting/ru/
---

В популярных окружениях рабочего стола (Gnome, KDE, XFCE) автомонтирование работает "из коробки". Для получения доступа к данным через Nautilus, Dolphin или Thunar нужно просто вставить флешку. Так как **awesome** это не окружение рабочего стола, а оконный менеджер, он не предоставляет такую возможность по умолчанию. Чтобы сделать это, существуют отдельные приложения (без зависимостей), которые осуществляют автоматическое монтирование очень хорошо, вы можете познакомится с этими приложениями ниже. ivman великолепно выполняет эту работу, и не имеет зависимостей, но кажется более не развивается с 2007, Halvet является его приемником.

Ivman
=====

[Ivman](http://ivman.sourceforge.net/)

В дистрибутивах, основанных на Debian, эту программу можно установить, набрав в консоли:

`  sudo apt-get install ivman`

В Archlinux ivman находится в AUR:

`  `[`http://aur.archlinux.org/packages.php?ID=18938`](http://aur.archlinux.org/packages.php?ID=18938)

Если у Вас FreeBSD, наберите в консоли:

`  su root -c pkg_add\ -r\ ivman`

В Gentoo Linux:

`  # emerge sys-apps/ivman`
`  # /etc/init.d/ivman start`

Или добавьте его на уровень запуска default:

`  # rc-update -a ivman default`

Не для Gentoo Linux: После установки просто добавьте [Ivman](http://ivman.sourceforge.net/) в файл *~/.xinitrc*. Пример:

`  #!/bin/sh`
`  ivman &`
`  exec awesome`

Или же просто запустите ivman и наслаждайтесь автомонтированием.

Halevt
======

Ivman, похоже, больше не разрабатывается (последняя версия вышла в 2007 году) и не работает с последними версиями hal и dbus. Его преемником является halevt [1](http://www.nongnu.org/halevt/). Формат конфигурационного файла halevt очень похож на используемый в ivman.

В Debian halevt доступен в ветке testing и устанавливается командой:

`   # aptitude install halevt`

Теперь сменные носители будут монтироваться автоматически. Если этого не происходит, возможно, потребуется добавить в /etc/PolicyKit/PolicyKit.conf что-нибудь вроде:

`   `<match user="halevt">
`               `<match action="org.freedesktop.hal.storage.mount-removable">
`                       `<return result="yes"/>
`               `</match>
`   `</match>

и перезапустить HAL.

Udisks и udisks-glue
====================

Я думаю, что это лучшее решение. Здесь используются [udisks](http://hal.freedesktop.org/releases/) и [udisks-glue](http://github.com/fernandotcl/udisks-glue). Синтаксис файла конфигурации очень прост, если вы пользовались ранее awesome 2.x, он вам понравится (libconfuse-base ;)).

Пример файла конфигурации udisks-glue: эта конфигурация генерирует уведомление каждый раз когда udisks-glue монтирует или размонтирует устройство:

` filter disks {`
`          optical = false`
`          partition_table = false`
`          usage = filesystem`
` }`
` match disks {`
`          automount = true`
`          automount_options = sync`
`          post_mount_command = "echo \'naughty.notify({title = \"USB:\", text =\"mounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
`          post_unmount_command = "echo \'naughty.notify({title = \"USB:\", text =\"unmounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
` }`
` filter optical {`
`         optical = true`
` }`
` match optical {`
`         automount = true`
`         automount_options = ro`
`         post_mount_command = "echo \'naughty.notify({title = \"CD-Rom:\", text =\"mounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
`         post_mount_command = "echo \'udisks_glue:mount_device(\"%device_file\",\"%mount_point\",\"Cdrom\")\' | awesome-client"`
`         post_unmount_command = "echo \'naughty.notify({title = \"CD-Rom:\", text =\"unmounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
` }`

Смотрите также [модуль blingbling основанный на меню для событий udisks-glue](/Blingbling/ru#Пример_меню_udisks-glue: "wikilink").

Autofs/udev/uam
===============

ivman (как и halevt) являются клиентами службы HAL и потому не могут работать без него. В случае, если по какой-либо причине использование HAL нежелательно, следует рассмотреть возможность использования udev и/или файловой системы autofs.

Autofs
------

Старый добрый мир программного обеспечения позволяет монтировать и размонтировать некоторые устройства автоматически. Единственным недостатком является названия точек монтирования. Если кто то знает универсальное решение по использованию автомонтирования, внесите пожалуйста свой вклад. Главным конфигурационным файлом является auto.master, который считывается при старте скриптом управления autofs. Демон запускается скриптом autofs - automount - должен работать постоянно, чтобы автомонтирование работало.

Autofs монтирует устройства по требованию, т.е. когда вы пытаетесь использовать авто завершение названия каталога или просто набираете полный путь к известному вам файлу на устройстве и пытаетесь получить к нему доступ. Autofs позволяет также размонтировать устройство, когда оно не используется какое то время . Из man autofs: Для каждой точки монтирования (в файле конфигурации) automount(8) будет монтировать и запускать поток с соответствующими параметрами, для управления этой точкой доступа.

Debian:

    # aptitude install autofs

Некоторые опции можно найти в /etc/default/autofs. Например, я изменил расположение файла auto.master на /etc/autofs/auto.master, так как мне хочется, чтобы autofs имел отдельный каталог. Скрипт находится в /etc/init.d/autofs

Arch Linux:

    # pacman -Sy autofs

Главный конфигурационный файл по умолчанию /etc/autofs/auto.maste. Скрипт располагается /etc/rc.d/autofs

Настраивайте. Читаетй man aswell ;)

auto.master

    +auto.master
    #As far as i know, any number of files from any location can be sourced from here.
    #automount anything described in auto.auto at /media, unmounting after 2 idle seconds.
    /media  /etc/autofs/auto.auto --timeout 2

auto.auto

    #linux      -ro,soft,intr       ftp.example.org:/pub/linux
    # all sd devices with the directory names as 2 last chars (b1, c1, e.g. /media/b1 for /dev/sdb1)
    *   -fstype=auto,async,nodev,nosuid,gid=100,uid=1000       :/dev/sd&
    #mounts devices/partitions with known names
    devname   -fstype=auto,async,nodev,nosuid,gid=100,uid=1000       LABEL="DEVNAME"
    my   -fstype=auto,async,nodev,nosuid,gid=100,uid=1000       LABEL="My Passport"
    # mount a single device
    cd  -fstype=auto,ro,nodev,nosuid :/dev/cdrom

Как видно из примеров, autofs также может автомонтировать сеть и т.д.

Udev
----

Udev может быть настроен на автомонтирование. Главное преимущество в том, что если у вас уже запущен udev, вам не понадобится никаких дополнительных демонов. Однако команда размонтирования может быть использована только под root пользователем. Одна из опций позволяет вам установить возможность использования синхронного трансфера, что позволяет безопасно удалить устройство по завершении, и размонтирует после его удаления; это может уменьшить срок службы некоторых флешек, хотя насколько мне известно, это не является проблемой для большинства современных флеш-устройств. Другим вариантом является использование pmount вместо mount. Примеры как это настроить вы можете найти на [the udev page on ArchWiki](https://wiki.archlinux.org/index.php/Udev#Auto_mounting_USB_devices).

Uam
---

[uam](https://wiki.archlinux.org/index.php/Udev#Auto_mounting_USB_devices) подскажет как настроит Udeb, что позволит вам использовать автомонтирование.

В gentoo:

`  # emerge uam pmount`
`  # usermod -aG plugdev username-you-want-to-be-able-to-use-automounted-things`
`  # /etc/init.d/udev restart`

Автомонтирование будет происходить в /media/$LABEL, и может быть размонтированно с помощью `uam-pumount` `/media/$LABEL`.

Udiskie
=======

При наличии udisks, автомонтирование можно обеспечить утилитой [udiskie](http://comapt.freecode.com/projects/udiskie), написанной на python. После запуска udiskie находит и автоматически монтирует разделы на съемных устройствах. Можно принудительно смонтировать и размонтировать раздел командами **udiskie-mount** и **udiskie-umount**.

В случае, если в системе установлен PyGTK, можно запустить udiskie с иконкой в трее при помощи ключа --icon

    awful.util.spawn("udiskie --tray")