---
title: Autostart zh
permalink: /Autostart/zh/
---

Awesome不提供自动启动功能；也不理会freedesktop的autostart标准和*\*.desktop*文件。你可以考虑如下几种方式。

传统方式
--------

在那些没或没有使用如GDM/KDM/XDM这样的登陆管理器的系统上使用**xinit**程序来启动X服务器和客户端程序。如果没有指定参数的话*xinit*会去寻找用户家目录下一个叫做**.xinitrc**的文件然后把它作为shell脚本执行。这个配置文件/脚本就用来启动客户端程序。

你可以在你的*~/.xinitrc*文件中运行任何程序但是要牢记将那些没有返回或退出的程序放到后台去执行，以免阻碍其他程序的启动。只有最后一个启动的程序，你的窗口管理器(当然就是awesome)，需要让它在前台运行从而使得脚本在你退出awesome之前不会退出，这样*xinit*就能够在脚本结束之后做一些清理工作。

如果你使用**startx**命令启动X服务器，不用担心这样对你有任何影响，*startx*命令仅仅是*xinit*的一个前端而已，你的*~/.xinitrc*文件会照常执行。示例*.xinitrc*文件相当简单：

` #!/bin/sh`
` #`
` # 用户 .xinitrc 文件`
` `
` # 合并自定义 X 资源`
` xrdb -merge "${HOME}/.Xresources"`
` `
` # 后台播放启动音乐`
` ogg123 -q "${HOME}/.config/awesome/login.ogg" &`
` `
` # 后台启动一个终端模拟器`
` urxvt -T Terminal &`
` `
` # 启动窗口管理器`
` exec awesome`

这是一个比较复杂的*.xinitrc*可以作为很好的学习示例：http://git.sysphere.org/dotfiles/tree/xinitrc

**注意：** 如果你使用*ck-launch-session*启动awesome，你也许想要创建独立的启动脚本来启动awesome或者你可以使用另一种方式来启动程序--从rc.lua文件中。相关问题：https://bbs.archlinux.org/viewtopic.php?pid=1085191

如果你使用登陆管理器，大多数会希望你使用窗口管理器来自动启动应用程序；继续阅读以了解如何使用awesome来达到这一目的。还有一些读取.xprofile和.xsession文件。gdm，kdm，和lightdm都使用.xprofile文件。由于在窗口管理器之前运行所以无法启动GUI应用程序但是可以启动一些后台进程或者设置一些环境变量--这都是当你没有使用登陆管理器时在.xinitrc中所做的。

Freedesktop自动启动方式
-----------------------

正如本页第一句所说awesome并没有实现Freedesktop的启动应用程序的方式，但这并不对你使用它产生任何阻碍。

The **dex** program interprets \*.desktop files in the locations specified by Freedesktop autostart documentation. Just run it like this via one of the ways described above/below:

`  /home/USER/bin/dex -a`

For specifying which applications shall be started use **gnome-session-properties** or similar programs from Gnome or KDE.

You should also be able to symlink autostart entries from /usr/share/applications into ~/.config/autostart. Dex will then be able to find your user-specific autostart apps and run them. Verify with:

`  dex -d -a`

Dex can be downloaded here: <http://github.com/jceb/dex>

Simple way
----------

Just add lines to end of your ~/.config/awesome/rc.lua:

    awful.util.spawn_with_shell("COMMAND1")
    awful.util.spawn_with_shell("COMMAND2")

...and so on

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
            awful.util.spawn_with_shell("pgrep -f -u $USER -x '" .. pname .. "' || (" .. prg .. " " .. arg_string .. ")",screen)
        end
    end

    run_once("xscreensaver","-no-splash")
    run_once("pidgin",nil,nil,2)
    run_once("wicd-client",nil,"/usr/bin/python2 -O /usr/share/wicd/gtk/wicd-client.py")

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

The native lua way
------------------

This solution doesn't depend on external tools, which speed up your startup.

To use this code snippet luafilesystem alias lfs is required.

It should be avaible for every system: debian (lua-filesystem), freebsd (ports/devel/luafilesystem/), gentoo (dev-lua/luafilesystem), ubuntu (liblua5.1-filesystem0), archlinux (luafilesystem)

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

[Category:Awesome3](/Category:Awesome3 "wikilink")