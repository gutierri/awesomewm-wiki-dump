---
title: Autostart ru
permalink: /Autostart/ru/
---

В Awesome отсутствует встроенная поддержка автозапуска приложений. Чтобы настроить автозапуск, Вы можете отредактировать либо .xinitrc, либо сессионный скрипт, выполняемый gdm или kdm, или же найти какой-то другой способ запускать приложения вместе с Awesome.

Обычный способ
--------------

**xinit** используется для запуска X сервера и приложений, на системах не имеющих вообще, или использующих login менеджеры, такие как GDM/KDM/XDM. При запуске *xinit* без специальных опций приложений, он ищет файл **.xinitrc** в домашнем каталоге и запускает его как shell скрипт. Этот конфигурационный файл/скрипт используется для запуска клиентских приложений.

Вы можете запускать любые приложения из вашего файла *~/.xinitrc*, но имейте в виду, что программы которые не завершаются или не должны завершаться, вы должны отправлять в фоновый режим работы, поскольку они не блокируют другие приложения при старте. Только последняя запускаемая программа, ваш оконный менеджер (в нашем случае awesome), должен запускаться на переднем плане (т.к. она блокирует дальнейшее выполнение скрипта), поэтому скрипт не завершается до вашего выхода из awesome, и поэтому *xinit* может произвести очистку, только после выхода.

Если вы используете запуск X сервера с помощью команды **startx**, и беспокоитесь на то как это влияет на запуск приложений, не беспокойтесь, команда *startx* считывает *xinit*, и ваш файл *~/.xinitrc* также будет выполнен. Простейший пример файла *.xinitrc* приведен ниже:

` #!/bin/sh`
` #`
` # User's .xinitrc file`
` `
` # Считывание пользовательского файла X resources`
` xrdb -merge "${HOME}/.Xresources"`
` `
` # Воспроизведение звука запуска в фоновом режиме`
` ogg123 -q "${HOME}/.config/awesome/login.ogg" &`
` `
` # Запуск эмулятора терминала в фоновом режиме`
` urxvt -T Terminal &`
` `
` # Запуск оконного менеджера`
` exec awesome`

Более сложный пример использования файла *.xinitrc* вы можете найти на здесь: <http://git.sysphere.org/dotfiles/tree/xinitrc>

**Примечание:** Если вы запускаете awesome используя *ck-launch-session*, вы можете захотеть создать раздельный старт скриптов и запуск awesome , или вы можете исплользовать альтернативный способ запуска приложений в rc.lua. Ссылка на этот способ: <https://bbs.archlinux.org/viewtopic.php?pid=1085191>

Если вы используете login manager, то вы можете использовать оконный менеджер для автозапуска приложений; продолжайте чтение, для изучения различных способов как это реализовать в awesome. Однако, какие то менеджеры считывают .xprofile, а другие .xsession. gdm, kdm, и lightdm используют .xprofile. Но поспольку он обрабатывается до запуска оконного менеджера, то вы не сможете использовать его для запуска графических приложений, зато этот способ отлично подходит для автозапуска фоновых приложений, или для установки переменных окружения, которые обычно прописываются в .xinitrc, когда вы не используете login manager.

Способ от Freedesktop
---------------------

Как уже говорилось выше, awesome не поддерживает способ запуска приложений Freedesktop, но это не должно мешать вам использовать его.

Программа **dex** позволяет обработать файлы \*.desktop расположенные в определенных спецификацией Freedesktop местах автозапуска. Просто запустите его, используюя способ описанный ниже:

`  dex -a -e Awesome`

Для определения запускаемых программ используйте **gnome-session-properties** или аналогичных программ из Gnome или KDE.

Вы также можете сделать симлинк(symlink) на список автозапуска из /usr/share/applications в ~/.config/autostart. После этого dex сможет найти определеные пользователем автоматически запускаемые приложения и запустить их. Проверьте следующий код:

`  dex -a -e Awesome -d`

Вы можете скачать Dex здесь: <http://github.com/jceb/dex>. Или можете установить его в Archlinux используя "pacman -S [dex](https://www.archlinux.org/packages/community/any/dex/)".

Простой способ
--------------

Просто добавьте несколько строк в конец файла ~/.config/awesome/rc.lua:

    awful.util.spawn_with_shell("КОМАНДА1")
    awful.util.spawn_with_shell("КОМАНДА2")

...и так далее.

**Пример:**

    awful.util.spawn_with_shell("kdeinit")
    awful.util.spawn_with_shell("lineakd")
    awful.util.spawn_with_shell("anyremote -f ~/.anyRemote/amarok.cfg")
    awful.util.spawn_with_shell("~/scripts/trm")
    awful.util.spawn_with_shell("xchat")
    awful.util.spawn_with_shell("psi")
    awful.util.spawn_with_shell("firefox-bin")
    awful.util.spawn_with_shell("gvim +Project")
    awful.util.spawn_with_shell("kchmviewer")
    awful.util.spawn_with_shell("amarok")
    awful.util.spawn_with_shell("kmix")
    awful.util.spawn_with_shell("kbluetoothd")
    awful.util.spawn_with_shell("sudo killall mplayer")

Чтобы запускать приложения только один раз, а не при каждом перезапуске Awesome, создайте такой скрипт:

    #! /bin/bash

    # Запустить программу, если она ещё не запущена.

    if [ -z "`ps -Af | grep -o -w ".*$1" | grep -v grep | grep -v run-once`" ]; then
      $@
    fi

Или так:

    #!/bin/bash
    #Alternative
    pgrep $@ > /dev/null || ($@ &)

Сохраните скрипт под именем "run_once" где-нибудь в $PATH и сделайте исполняемым. Затем используйте его для автозапуска:

    awful.util.spawn_with_shell("run_once amarok")

В качестве альтернативы отдельному shell-скрипту можно добавить в rc.lua следующий код (который к тому же будет игнорировать приложения, запущенные другими пользователями):

    function run_once(cmd)
      findme = cmd
      firstspace = cmd:find(" ")
      if firstspace then
        findme = cmd:sub(0, firstspace-1)
      end
      awful.util.spawn_with_shell("pgrep -u $USER -x " .. findme .. " > /dev/null || (" .. cmd .. ")")
    end

    run_once("amarok")
    run_once("xscreensaver -no-splash")

Или более продвинутую версию, которая позволяет использовать опции командной строки и позволяющая указать, на каком экране запускать приложение. Этот код также позволяет коректно обрабатывать ситуации, когда запускаемое приложение и имя процесса различаются (например wicd-client).

    function run_once(prg,arg_string,pname,screen)
        if not prg then
            do return nil end
        end

        if not pname then
           pname = prg
        end

        if not arg_string then
            awful.util.spawn_with_shell("pgrep -f -u $USER -x '" .. pname .. "' || (" .. prg .. ")",screen)
        else
            awful.util.spawn_with_shell("pgrep -f -u $USER -x '" .. pname .. " ".. arg_string .."' || (" .. prg .. " " .. arg_string .. ")",screen)
        end
    end

    run_once("xscreensaver","-no-splash")
    run_once("pidgin",nil,nil,2)
    run_once("wicd-client",nil,"/usr/bin/python2 -O /usr/share/wicd/gtk/wicd-client.py")

С друкой стороноы, если вы захотите запустить несколько экземпляров терминала с различными именами,это также возможно используя способ указанный выше, но это потребует дополнительной установки wmctrl.

Сохраните этот маленький скрипт, где нибудь в указанном в $PATH каталоге и сделайте его исполняемым:

    #!/bin/bash
    EXPECTED_ARGS=2

    if [ $# -ne $EXPECTED_ARGS ]
    then
      exit 1
    fi

    COUNT=`wmctrl -l -x | grep $1 | wc -l`

    if [ "$COUNT" -lt "$2" ]
    then
      exit 0
    else
      exit 1
    fi

В этом случае команды автозапуска будут выглядеть следующим образом. Используя *name="SSH"* и *name="log* вы будете размещать их в разных тегах:

    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited log 2 && urxvt -name log");
    awful.util.spawn_with_shell("run_limited log 2 && urxvt -name log");

Способ, использующий директорию автозапуска
-------------------------------------------

    -- Автозапуск
    function autostart(dir)
        if not dir then
            do return nil end
        end
        local fd = io.popen("ls -1 -F " .. dir)
        if not fd then
            do return nil end
        end
        for file in fd:lines() do
            local c= string.sub(file,-1)   -- последний символ
            if c=='*' then  -- исполняемые файлы
                executable = string.sub( file, 1,-2 )
                print("Автозапуск Awesome. Запускается: " .. executable)
                awful.util.spawn_with_shell(dir .. "/" .. executable .. "") -- запуск в фоне
            elseif c=='@' then  -- символические ссылки
                print("Автозапуск Awesome. Симнолические ссылки пропускаются: " .. file)
            else
                print ("Автозапуск Awesome. Игнорируем файл " .. file .. " , т.к. не является исполняемым.")
            end
        end
        io.close(fd)
    end

    autostart_dir = os.getenv("HOME") .. "/.config/autostart"
    autostart(autostart_dir)

Учтите (и по возможности исправьте) следующие недостатки:

-   файлы в директории автозапуска будут выполняться при каждом чтении конфигурационного файла, что может привести к неожиданным последствиям, если Вы делаете это несколько раз за сеанс. Чтобы избежать повторных запусков, обрабатывайте их в соответствующем скрипте.
-   функция игнорирует символьные ссылки.

Способ использующий идентификаторы(PID)
---------------------------------------

Поместите следующий код в runonce.lua

    -- @автор Peter J. Kranz (Absurd-Mind, peter@myref.net)
    -- Любые вопросы, критику или благодарности отправляйте на электронную почту

    local M = {}

    -- получаем текущий идентификатор(Pid) awesome
    local function getCurrentPid()
        -- получаем идентификатор awesome используя pgrep
        local fpid = io.popen("pgrep -u " .. os.getenv("USER") .. " -o awesome")
        local pid = fpid:read("*n")
        fpid:close()

        -- проверка корректности
        if pid == nil then
            return -1
        end

        return pid
    end

    local function getOldPid(filename)
        -- открыаем файл
        local pidFile = io.open(filename)
        if pidFile == nil then
            return -1
        end

        -- считываем колличество
        local pid = pidFile:read("*n")
        pidFile:close()

        -- проверка на то, что колличество больше 0
        if pid <= 0 then
            return -1
        end

        return pid;
    end

    local function writePid(filename, pid)
        local pidFile = io.open(filename, "w+")
        pidFile:write(pid)
        pidFile:close()
    end

    local function shallExecute(oldPid, newPid)
        -- простая проверка на равенство
        if oldPid == newPid then
            return false
        end

        return true
    end

    local function getPidFile()
        local host = io.lines("/proc/sys/kernel/hostname")()
        return awful.util.getdir("cache") .. "/awesome." .. host .. ".pid"
    end

    -- запускаем один раз при первом запуске awesome (настройка перезапуска работает)
    -- не распростраянется на "pkill awesome && awesome"
    function M.run(shellCommand)
        -- проверяем и запускаем
        if shallExecute(M.oldPid, M.currentPid) then
            awful.util.spawn_with_shell(shellCommand)
        end
    end

    M.pidFile = getPidFile()
    M.oldPid = getOldPid(M.pidFile)
    M.currentPid = getCurrentPid()
    writePid(M.pidFile, M.currentPid)

    return M

Для запуска используем следующий код:

    local r = require("runonce")  -- в начало файла

    r.run("urxvtd -q -o -f")
    r.run("urxvtc")
    r.run("urxvtc")
    r.run("wmname LG3D")

**Примечание:** runonce.lua будет работать только если оконный менеджер запущен напрямую, как "awesome". Но, если он запущен через симлинк (например,как x-window-manager в Debian), то он будет работать некорректно. В качестве альтернативы, замените часть `--get` `awesome` `pid` `from` `pgrep` на получение его PID непосредственно из /proc а не полагайтесь на pgrep с его нестабильными именами:

    -- get awesome pid from /proc
    local fpid = assert(io.open("/proc/self/stat", "r"))
    local pid = fpid:read("*all")
    fpid:close()
    pid = string.match(t, "%S+")

Встроенный способ lua
---------------------

Этот способ не зависит от внешних утилит, которые ускоряют запуск системы.

Для использования этого кода lua-filesystem требуется lfs.

Он должен быть доступн для большинства систем: debian (lua-filesystem), freebsd (ports/devel/luafilesystem/), gentoo (dev-lua/luafilesystem), ubuntu (liblua5.1-filesystem0), archlinux (luafilesystem)

@bsdguys: не забудьте смонтировать procfs (:

    require("lfs")
    -- {{{ Зупускаем программу единожны(run-once)
    local function processwalker()
       local function yieldprocess()
          for dir in lfs.dir("/proc") do
            -- Все каталоги в /proc содержат число, представляющее процесс
            if tonumber(dir) ~= nil then
              local f, err = io.open("/proc/"..dir.."/cmdline")
              if f then
                local cmdline = f:read("*all")
                f:close()
                if cmdline ~= "" then
                  coroutine.yield(cmdline)
                end
              end
            end
          end
        end
        return coroutine.wrap(yieldprocess)
    end

    local function run_once(process, cmd)
       assert(type(process) == "string")
       local regex_killer = {
          ["+"]  = "%+", ["-"] = "%-",
          ["*"]  = "%*", ["?"]  = "%?" }

       for p in processwalker() do
          if p:find(process:gsub("[-+?*]", regex_killer)) then
         return
          end
       end
       return awful.util.spawn(cmd or process)
    end
    -- }}}

    -- Пример использования
    run_once("firefox")
    run_once("dropboxd")
    -- Use the second argument, if the programm you wanna start,
    -- differs from the what you want to search.
    run_once("redshift", "nice -n19 redshift -l 51:14 -t 5700:4500")

Способ X сервера
----------------

Еще одним способом проверить запущен ли процесс, является проверка наличия у него окна. Эта возоможность работает только для процессов подключенных к X server. Есть два недостатка у этого подхода. Первый, awesome не имеет списка всех приложений, только тех приложений, которые имеют постоянное окно. Если приложение не имеет окна (например приложения в трее), его не будет в списке awesome. Второй недостаток, заключается в том, что список приложений будет доступен только после полного старта awesome, т.е. во время инициализации, он не доступен.

Обойти первую проблему можно используя (как вариант) xwininfo. Вторую проблему можно решить используя таймер. Вот код:

    local xrun_now = function(name, cmd)
       -- Сначала получаем список приложения из awesome (которые доступны
       -- только  если awesome уже полностью запущен, поэтому, эта функция
       -- должна запускаться внутри 0 таймера)
       local squid = { name, name:sub(1,1):upper() .. name:sub(2) }
       if awful.client.cycle(
          function(c)
         return awful.rules.match_any(c,
                          { name = squid,
                        class = squid,
                        instance = squid })
          end)() then
          return
       end

       -- Не найденно, проверяем с помощью xwininfo. Мы можем получить приложения имеющие имя,
       -- но мы не получим приложения без окон...
       if os.execute("xwininfo -name '" .. name .. "' > /dev/null 2> /dev/null") == 0 then
          return
       end
       awful.util.spawn_with_shell(cmd or name)
    end

    -- Выполняем команду, если еще не запушена.
    xrun = function(name, cmd)
       -- Нам нужно дождаться когда awesome полностью запустится. Поэтому используем таймер.
       local stimer = timer { timeout = 0 }
       local run = function()
          stimer:stop()
          xrun_now(name, cmd)
       end
       stimer:add_signal("timeout", run)
       stimer:start()
    end

Пример использования:

    xrun("chromium")
    xrun("pidgin", "pidgin -n")

Первый аргумента, это название программы (название окна, экземпляр объекта или класс. Второй аргумент, это команда запуска. Первый параметр используется как команда, если второй не определен.

Способ использующий файлы X конфигурации
----------------------------------------

Этот скрипт устанавливает свойства XResource отображающее, что приложение уже запущено. Этот метод по моему мнению является самым надежным. XResources существует только в памяти, поэтому необходимость в очистке отсутсвует до тех пор пока существует Х сессия.

Этот код также перенаправляет вывод всех запущенных процессов dex во временный файл вместо ~/.xsession-errors.

    #!/bin/sh

    PROP=autostart.wasrun
    HASPROP=$(xrdb -query|grep $PROP)

    if [ "$HASPROP" != "" ]
    then
      exit 0;
    fi

    TEMPLATE=xsession-$USER-$PPID-XXX
    TEMPFILE=$(mktemp --tmpdir $TEMPLATE)

    echo "logging to $TEMPFILE"

    echo "$PROP: on" | xrdb -merge

    ( $HOME/bin/dex -v -a 2>&1 1>$TEMPFILE ) &

У меня нет глубоких познаний в X resources, поэтому было бы неплохо, если бы кто нибудь проверил этот код. Похожий способ используется для запуска в lua вместо оболочки:

    xrun = function(cmd, late)
        local xresources = awful.util.pread("xrdb -query")
        local myxr = xresources:match("^awesome%.xrun:%s+.-\n")
                     or xresources:match("\nawesome%.xrun:%s+.-\n")
                     or "awesome.xrun:"
        myxr = myxr:gsub("\n", "")
        if not string.find(myxr, cmd, 1, true) then
            -- use pread so that this runs synchronously
            awful.util.pread("echo '" .. myxr .. " " .. cmd:gsub("'", "\\'") .. "'|xrdb -merge")
            if late then
                local stimer = timer { timeout = late }
                local run = function()
                    stimer:stop()
                    awful.util.spawn(cmd, false)
                end
                stimer:connect_signal("timeout", run)
                stimer:start()
            else
                awful.util.spawn(cmd, false)
            end
        end
    end

Или используйте более базовый метод для старта приложений при запуске.


    local xresources_name = "awesome.started"
    local xresources = awful.util.pread("xrdb -query")
    if not xresources:match(xresources_name) then
        -- Execute once for X server
        os.execute("dex -a -e Awesome")
    end
    awful.util.spawn_with_shell("xrdb -merge <<< " .. "'" .. xresources_name .. ": true'")

[Category:Awesome3](/Category:Awesome3 "wikilink")