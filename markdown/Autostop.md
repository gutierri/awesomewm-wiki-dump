---
title: Autostop
permalink: /Autostop/
---

There is no hook to execute a lua function at exit; but it is possible to connect a function as 'exit' signal handler.

The exit signal is not only called on real exit, but naturally on restart as well - so this is without additional logic not as practicable as it sounds.

But it's worth experimenting with.

DBus 'exit' signal handler
--------------------------

Add the following line to your rc.lua

` awesome.add_signal("exit", function() awful.util.spawn("atexit.sh") end)`

If you're using Archlinux, you should try the following line instead.

` awesome.connect_signal("exit", function () awful.util.spawn("atexit.sh") end)`

This will execute 'atexit.sh', if such an executable is found in your path. Of course you can do everything in the lua function itself, or spawn another program. Use your imagination :-).

I use this to stop all the daemons/services I start via dex as described in [Autostart](/Autostart "wikilink").

Tested with awesome 3.4.8

Alternative: Replace the Quit command
-------------------------------------

I was experimenting with the DBus solution above and I ran into one small issue, It would exit too fast to complete the scripts I wished to run.

However I found a potential solution. I changed one line in my rc.lua from:

`    awful.key({ modkey, "Shift" }, "q" awesome.quit),`

to:

`    awful.key({ modkey, "Shift" }, "q", function awful.util.spawn("konsole -e bb.sh") end),`

Then in bb.sh, I ran what I wished and then exited awesome like so:

`    #!/bin/bash`
`    bleachbit --delete --overwrite adobe_reader.cache adobe_reader.mru adobe_reader.tmp bash.history elinks.history firefox.cache firefox.cookies firefox.dom firefox.download_history firefox.forms firefox.session_restore firefox.site_preferences firefox.url_history firefox.vacuum flash.cache flash.cookies kde.cache kde.recent_documents kde.tmp konqueror.cookies konqueror.current_session konqueror.url_history links2.history nautilus.history realplayer.cookies realplayer.history realplayer.logs skype.chat_logs system.cache system.clipboard system.desktop_entry system.recent_documents system.tmp system.trash thumbnails.cache vim.history`
`    echo ; echo`
`    sudo bleachbit --delete --overwrite apt.autoclean apt.autoremove apt.clean system.localizations system.rotated_logs system.tmp system.trash x11.debug_logs`
`    # exit awesome`
`    echo 'awesome.quit()' | awesome-client`
`    # exit script`
`    exit 0`

So far, it works and it even pauses for me to enter the sudo password.

Another advantage is that it does not run the script if I trigger a restart, but rather only when I intend to quit and exit.

Tested with Awesome 3.4.10 on Debian -- lowkey