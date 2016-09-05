---
title: Autostart
permalink: /Autostart/
---

Awesome does not provide autostart functionality; not in the sense of freedesktop autostart specification and using *\*.desktop* files. Below are some solutions for you to consider.

Traditional way
---------------

The **xinit** program is used to start the X server and clients on systems that don't have, or use, a display login manager like GDM/KDM/XDM. When *xinit* is run without specific client options it looks for a file called **.xinitrc** in the user's home directory and runs it as a shell script. This configuration file/script is used to start up client programs.

You can run any application from your *~/.xinitrc* file but keep in mind that programs which do not return or exit right way you need to send to the background, so they don't block other programs from starting. Only the last started program, your window manager (awesome in this case), should be left in the foreground so the script doesn't exit until you quit awesome, and so *xinit* can clean up after it.

If you are used to start the X server with the **startx** command, and by now wondering how this will affect you, do not worry the *startx* command is only a front-end to *xinit*, and your *~/.xinitrc* file will also be executed. Very simple *.xinitrc* file as an example:

` #!/bin/sh`
` #`
` # User's .xinitrc file`
` `
` # Merge custom X resources`
` xrdb -merge "${HOME}/.Xresources"`
` `
` # Play a startup sound, in the background`
` ogg123 -q "${HOME}/.config/awesome/login.ogg" &`
` `
` # Start a terminal emulator in the background`
` urxvt -T Terminal &`
` `
` # Start the window manager`
` exec awesome`

Here is a good example of a more complex *.xinitrc* file you can learn from: <http://git.sysphere.org/dotfiles/tree/xinitrc>

**Note:** If you start awesome with *ck-launch-session*, you might want to create a separate start script and exec awesome from there or you can use an alternative way of starting the apps from inside rc.lua. Link to a related issue: <https://bbs.archlinux.org/viewtopic.php?pid=1085191>

If you are using a login manager, most will expect you to use the window manager to autostart applications; continue reading for various methods to do that with awesome. However some read .xprofile and some read .xsession. gdm, kdm, and lightdm all use .xprofile. It is run before the window manager is started so it can't be used to start graphical applications but it is a good way autostart background things or set environment variables that you would normally do in .xinitrc if you weren't using a login manager.

Freedesktop autostart way
-------------------------

As stated in the first sentence on this page awesome does not implement the Freedesktop way of spawning applications at startup but this shouldn't hinder you to use it anyway.

The **dex** program interprets \*.desktop files in the locations specified by Freedesktop autostart documentation. Just run it like this via one of the ways described above/below:

`  dex -a -e Awesome`

For specifying which applications shall be started use **gnome-session-properties** or similar programs from Gnome or KDE.

You should also be able to symlink autostart entries from /usr/share/applications into ~/.config/autostart. Dex will then be able to find your user-specific autostart apps and run them. Verify with:

`  dex -a -e Awesome -d`

Dex can be downloaded here: <http://github.com/jceb/dex>. Or can be installed directly from Archlinux by running "pacman -S [dex](https://www.archlinux.org/packages/community/any/dex/)".

Simple way
----------

Just add lines to end of your ~/.config/awesome/rc.lua:

    awful.util.spawn_with_shell("COMMAND1")
    awful.util.spawn_with_shell("COMMAND2")

**My Example:**

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

If you want to run your apps only once and not every time awesome is restarted, create this simple script:

    #! /bin/bash

    # Run program unless it's already running.

    if [ -z "`ps -Af | grep -o -w ".*$1" | grep -v grep | grep -v run-once`" ]; then
      $@
    fi

Or this:

    #!/bin/bash
    #Alternative
    pgrep $@ > /dev/null || ($@ &)

Save it as "run_once" somewhere in your $PATH and make it executable. Then autostart your apps like this:

    awful.util.spawn_with_shell("run_once amarok")

Alternatively you can use the following to avoid an external script (this also ignore commands running as other users than yourself):

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

Or this slightly more advanced version which permits to use command line options and to specify on which screen to launch your programs. It also allows for the case when the name of the process is different from the name of the command used to launch it (e.g. with wicd-client).

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

Alternatively, if you want to start multiple instances of terminal with different window names, it is possible to use a variation of the above, though this also requires installation of wmctrl.

Save this small bash file somewhere on your $PATH and make it executable:

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

The autostart commands will look something like this. Usually with rules to place name="SSH" on a different tag from name="log":

    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited SSH 4 && urxvt -name SSH");
    awful.util.spawn_with_shell("run_limited log 2 && urxvt -name log");
    awful.util.spawn_with_shell("run_limited log 2 && urxvt -name log");

Directory way
-------------

    -- Autostart
    function autostart(dir)
        if not dir then
            do return nil end
        end
        local fd = io.popen("ls -1 -F " .. dir)
        if not fd then
            do return nil end
        end
        for file in fd:lines() do
            local c= string.sub(file,-1)   -- last char
            if c=='*' then  -- executables
                executable = string.sub( file, 1,-2 )
                print("Awesome Autostart: Executing: " .. executable)
                awful.util.spawn_with_shell(dir .. "/" .. executable .. "") -- launch in bg
            elseif c=='@' then  -- symbolic links
                print("Awesome Autostart: Not handling symbolic links: " .. file)
            else
                print ("Awesome Autostart: Skipping file " .. file .. " not executable.")
            end
        end
        io.close(fd)
    end

    autostart_dir = os.getenv("HOME") .. "/.config/autostart"
    autostart(autostart_dir)

Be aware of the following drawbacks (and maybe fix them):

-   the files in the autostart directory will be run everytime you re-parse the config file. If you re-parse the config file multiple times during a session this might cause strange behavior. If an application should only be run once, please handle this in the corresponding autostart script.
-   The function currently ignores symlinks (I do not need them).

PID way
-------

Put this in runonce.lua

    -- @author Peter J. Kranz (Absurd-Mind, peter@myref.net)
    -- Any questions, criticism or praise just drop me an email

    local M = {}

    -- get the current Pid of awesome
    local function getCurrentPid()
        -- get awesome pid from pgrep
        local fpid = io.popen("pgrep -u " .. os.getenv("USER") .. " -o awesome")
        local pid = fpid:read("*n")
        fpid:close()

        -- sanity check
        if pid == nil then
            return -1
        end

        return pid
    end

    local function getOldPid(filename)
        -- open file
        local pidFile = io.open(filename)
        if pidFile == nil then
            return -1
        end

        -- read number
        local pid = pidFile:read("*n")
        pidFile:close()

        -- sanity check
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
        -- simple check if equivalent
        if oldPid == newPid then
            return false
        end

        return true
    end

    local function getPidFile()
        local host = io.lines("/proc/sys/kernel/hostname")()
        return awful.util.getdir("cache") .. "/awesome." .. host .. ".pid"
    end

    -- run Once per real awesome start (config reload works)
    -- does not cover "pkill awesome && awesome"
    function M.run(shellCommand)
        -- check and Execute
        if shallExecute(M.oldPid, M.currentPid) then
            awful.util.spawn_with_shell(shellCommand)
        end
    end

    M.pidFile = getPidFile()
    M.oldPid = getOldPid(M.pidFile)
    M.currentPid = getCurrentPid()
    writePid(M.pidFile, M.currentPid)

    return M

Use it this way:

    local r = require("runonce")

    r.run("urxvtd -q -o -f")
    r.run("urxvtc")
    r.run("urxvtc")
    r.run("wmname LG3D")

**NOTE:** the runonce.lua will only work if the window manager is started as "awesome". If it is started from a symlink (say, as x-window-manager on Debian), it will fail. An alternative is to replace the `--get` `awesome` `pid` `from` `pgrep` part to retrieve its PID directly from /proc instead of relying on pgrep and an unstable name:

    -- get awesome pid from /proc
    local fpid = assert(io.open("/proc/self/stat", "r"))
    local pid = fpid:read("*all")
    fpid:close()
    pid = string.match(t, "%S+")

The native lua way
------------------

This solution doesn't depend on external tools, which speed up your startup.

To use this code snippet luafilesystem alias lfs is required.

It should be avaible for every system: debian (lua-filesystem), freebsd (ports/devel/luafilesystem/), gentoo (dev-lua/luafilesystem), ubuntu (liblua5.1-filesystem0), archlinux (lua-filesystem)

@bsdguys: don't forget to mount procfs (:

    require("lfs")
    -- {{{ Run programm once
    local function processwalker()
       local function yieldprocess()
          for dir in lfs.dir("/proc") do
            -- All directories in /proc containing a number, represent a process
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

    -- Usage Example
    run_once("firefox")
    run_once("dropboxd")
    -- Use the second argument, if the programm you wanna start,
    -- differs from the what you want to search.
    run_once("redshift", "nice -n19 redshift -l 51:14 -t 5700:4500")

The X way
---------

Another way to detect if a process is already running is to check if there is a window for it. This only works for processes that connect to the X server. There are two drawbacks with this approach. First, awesome does not maintain a list of all clients, just for clients that have regular windows. If a client is window-less (for example, a systray icon), it is not listed by awesome. Second, the list of clients is only available once awesome has started completely, not during the initialization.

We can circumvent the first problem by using (as a fallback) xwininfo. The second problem is worked around with a timer. Here is the code:

    local xrun_now = function(name, cmd)
       -- Try first the list of clients from awesome (which is available
       -- only if awesome has fully started, therefore, this function
       -- should be run inside a 0 timer)
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

       -- Not found, let's check with xwininfo. We can only check name but
       -- we can catch application without a window...
       if os.execute("xwininfo -name '" .. name .. "' > /dev/null 2> /dev/null") == 0 then
          return
       end
       awful.util.spawn_with_shell(cmd or name)
    end

    -- Run a command if not already running.
    xrun = function(name, cmd)
       -- We need to wait for awesome to be ready. Hence the timer.
       local stimer = timer { timeout = 0 }
       local run = function()
          stimer:stop()
          xrun_now(name, cmd)
       end
       stimer:add_signal("timeout", run)
       stimer:start()
    end

Example of use:

    xrun("chromium")
    xrun("pidgin", "pidgin -n")

First argument is the name of the program (name of window, instance or class, it will also be searched capitalized). The second is the command to run. The first argument is used as a command if no second argument is provided.

The X Resources way
-------------------

This script sets an XResource property to indicate that autorun was already run. This method is in my opinion the most reliable one. XResources exist only in RAM so no cleanup is necessary and XResources exist for as long as the X session exist.

The code also redirects the output of all methods started by dex to a tempfile instead of ~/.xsession-errors.

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

I have no deeper knowledge on X resources so I'd be happy if somebody would like to review this.

This is a similar approach that runs in lua instead of shell:

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

Or using a more general method to do anything on start but not restart

    local xresources_name = "awesome.started"
    local xresources = awful.util.pread("xrdb -query")
    if not xresources:match(xresources_name) then
        -- Execute once for X server
        os.execute("dex -a -e Awesome")
    end
    awful.util.spawn_with_shell("xrdb -merge <<< " .. "'" .. xresources_name .. ": true'")

-   Wouldn't it be nice if awesome lua would have functions to manipulate and query X resources?

<!-- -->

-   Wouldn't it be nice if awesome would by itself already set an X resource property once it has completed the processing of its configuration? Later restarts could then check whether this property is already set.

<!-- -->

-   Wouldn't it be nice if there'd actually be some free desktop standard property to be set once a window manager has fully started up?

[Category:Awesome3](/Category:Awesome3 "wikilink")