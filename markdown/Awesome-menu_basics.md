---
title: Awesome-menu basics
permalink: /Awesome-menu_basics/
---

awesome-menu basics
-------------------

In awesome 2.3 (now outdated), there's a menu mechanism. There's two sections to the menu, the first is the command being ran, and the rest of the bar is the menu. The first portion can take input from stdin, so any command can be typed there instead of using a menu option.

To add a list of items to the menu, you can cat a file of contents to it as shown in the following:

~/.awesome/menu contents:

` xterm`
` firefox`
` pidgin`

Then add this to your .awesomerc:

` key { `
`   modkey = {"Mod4"} `
`   key = "p" `
`   command = "spawn" `
``    arg = "exec `cat ~/.awesome/menu | awesome-menu 'Run:'`"  ``
` }`

Or, another alternative, which displays everything in /usr/bin as a menu:

` key { `
`   modkey = {"Mod4"} `
`   key = "p" `
`   command = "spawn" `
`   arg = "ls /usr/bin | awesome-menu -e 'exec ' 'Run:'" `
` }`

Or, you can display everything in $PATH as a menu with dmenu script

` key { `
`   modkey = {"Mod4"} `
`   key = "p" `
`   command = "spawn" `
`   arg = "dmenu_path | awesome-menu -e 'exec ' 'Run:'" `
` }`

PS If you dont have dmenu installed, there is dmenu_path script

`#!/bin/sh`
`CACHE=$HOME/.dmenu_cache`
`IFS=:`
`uptodate() {`
`test ! -f $CACHE && return 1`
`for dir in $PATH`
`do`
`      test $dir -nt $CACHE && return 1`
`done`
`return 0`
`}`
`if ! uptodate`
`then`
`       for dir in $PATH`
`       do`
`               for file in "$dir"/*`
`               do`
`                       test -x "$file" && echo "${file##*/}"`
`               done`
`       done | sort | uniq > $CACHE.$$`
`       mv $CACHE.$$ $CACHE`
`fi`
`cat $CACHE`

Stylizing awesome-menu
----------------------

The awesome-menu stylized using the .awesomerc. When calling awesome-menu, a label can be defined, and awesome-menu uses this to stylize the menu.

` menu Run:  `
` {`
`   styles`
`   {`
`       normal { `
`         bg = "#0a0a0a" `
`         fg = "#a0a0a0" `
`         shadow = "#111111" `
`         shadow_offset = "1" `
`       }`
`       focus { `
`         bg = "#285577" `
`         fg ="#ffffff" `
`         shadow = "#111111" `
`         shadow_offset = "1" `
`       }`
`   } `
`   y = "0"`
`   x = "180"`
`   height = "14"`
` }`

Slightly more advanced awesome-menu
-----------------------------------

Of course, the methods shown above don't look so nice when you want to run 'xterm -bg yellow -fg white', so a simple shell script wrapper can be written:

` #!/bin/bash`
` ####`
` #`
` # awesome-menu-builder - Simple shell script to build menus for `
` # awesome-menu.`
` #`
` # Concept from `[`http://www.calmar.ws/tmp/dmenu_do`](http://www.calmar.ws/tmp/dmenu_do)
` #`
` ####`
` `
` # build your menu here with whatever simple names you want to use`
` MENU="rxvt`
` firefox`
` pidgin`
` xfe`
` vim`
` mutt`
` logout`
` "`
` # display the menu and yank the choice made`
` CHOICE=$(awesome-menu <<< "${MENU}" "Run: ")`
` # parse the choice`
` case ${CHOICE%% *} in`
`     # you only need to put lines here for what you want to run`
`     # differently than the rest`
`     rxvt)`
`         awesome-client <<< "0 spawn rxvt -fg white -bg black"`
`     ;;`
`     logout)`
`         awesome-client <<< "0 quit"`
`     ;;`
`     vim|mutt)`
`         awesome-client <<< "0 spawn rxvt -fg white -bg black -e '${CHOICE}'"`
`     ;;`
`     *)`
`        awesome-client <<< "0 spawn ${CHOICE}"`
`     ;;`
` esac`

An even more advanced awesome-menu
----------------------------------

Copy this script (name to something like awesome_launch), modify the command sets as you please, then use as follows:

` key `
` {   `
`     modkey = {"Mod1"}`
`     key = "F11"`
`     command = "spawn"`
`     arg = "exec awesome_launch prog"`
` }`
` key `
` {   `
`     modkey = {"Mod1"}`
`     key = "F12"`
`     command = "spawn"`
`     arg = "exec awesome_launch mpc"`
` }`

` #!/usr/bin/python`
` `
` import os, sys, shlex, commands`
` `
` #rxvt = 'urxvt';`
` rxvt = 'urxvtc';`
` #rxvt = 'rxvt';`
` `
` # cmd set sort directive`
` YES_SORT = True`
` NO_SORT = False`
` `
` # cmd set field index`
` SORT = 0`
` TEXT = 1`
` CMDS = 2`
` `
` cmdSet = \`
` {`
`   "prog":`
`   [`
`     YES_SORT,`
`     "Whazup",`
`     [`
`       [ "rxvt",    rxvt + " -title rxvt -tr -tint black -sh 50 -e bash -li" ],`
`       [ "scr",     rxvt + " -title screen +sb -e screen -dR" ],`
`       [ "remote",  rxvt + " -title insanum +sb -tr -tint black -sh 50 -e ssh -Y -t foo\@bar.com screen -dR" ],`
`       [ "mocp",    rxvt + " -title mocp -e mocp" ],`
`       [ "fox",     "firefox" ],`
`       [ "fox3",    "firefox3" ],`
`       [ "opera",   "opera" ],`
`       [ "wire",    "sudo wireshark" ],`
`       [ "xchm",    "xchm" ],`
`       [ "xpdf",    "xpdf" ],`
`       [ "acro",    "acroread" ],`
`       [ "emel",    "emelfm2" ],`
`       [ "rpd",     "rdesktop -T windows -g 80% -K -0 -u Administrator -p whatever somehost" ],`
`     ]`
`   ],`
` `
`   "mpc":`
`   [`
`     NO_SORT,`
`     "MPC",`
`     [`
`       [ "play",   "mpc play" ],`
`       [ "stop",   "mpc stop" ],`
`       [ "next",   "mpc next" ],`
`       [ "prev",   "mpc prev" ],`
`       [ "pause",  "mpc toggle" ],`
`       [ "repeat", "mpc repeat" ],`
`       [ "random", "mpc random" ],`
`       [ "sf",     "mpc seek +10" ],`
`       [ "sb",     "mpc seek -10" ],`
`       [ "info",   "mpc | dzen2 -l 5 -p -w 500 -ta l -bg darkblue -fg yellow -x 100 -y 100 -e 'onstart=uncollapse;button1=exit;button2=exit;button3=exit;'" ],`
`       [ "ncmpc",  rxvt + " -title mpc -e ncmpc" ],`
`       [ "sonata", "sonata" ],`
`       [ "gmpc",   "gmpc" ],`
`     ]`
`   ],`
` `
`   #"mocp":`
`   #[`
`   #  NO_SORT,`
`   #  "MOCP",`
`   #  [`
`   #    [ "play",  "mocp --play" ],`
`   #    [ "stop",  "mocp --stop" ],`
`   #    [ "next",  "mocp --next" ],`
`   #    [ "prev",  "mocp --previous" ],`
`   #    [ "pause", "mocp --toggle-pause" ],`
`   #    [ "info",  "mocp --info | dzen2 -l 10 -p -w 1000 -ta l -bg darkblue -fg yellow -x 100 -y 100 -e 'onstart=uncollapse;button1=exit;button2=exit;button3=exit;'" ],`
`   #    [ "exit",  "mocp --exit" ],`
`   #    [ "mocp",  rxvt + " -title mocp -e mocp" ],`
`   #  ]`
`   #],`
` }`
` `
` if len(sys.argv) != 2:`
`     sys.exit(1)`
` `
` set = sys.argv[1]`
` `
` if cmdSet[set][SORT] == YES_SORT:`
`     cmdSet[set][CMDS].sort(key=lambda x: x[0])`
` `
` cmdNames = ''`
` for c in cmdSet[set][CMDS]:`
`    cmdNames += c[0] + '\n'`
` `
` pipe = os.popen('echo -n -e "' + cmdNames + '" | awesome-menu "' + cmdSet[set][TEXT] + '"', 'r')`
` name = pipe.read().strip().rstrip()`
` pipe.close()`
` `
` for c in cmdSet[set][CMDS]:`
`     if c[0] == name:`
`         #os.system(c[1])`
`         tmp = shlex.split(c[1])`
`         pid = os.fork()`
`         if not pid:`
`             os.execvp(tmp[0], tmp)`

awesome-menu with history
-------------------------

This script makes the menu store history in ~/.awesome-history, and can include all the commands from the debian menu system too.

<http://git.kitenet.net/?p=joey/home.git;a=blob;f=bin/awesome-prompt>

[Category:Awesome2](/Category:Awesome2 "wikilink")