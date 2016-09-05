---
title: Awesome-3.4.11 git-Ubuntu-Lucid
permalink: /Awesome-3.4.11/git-Ubuntu-Lucid/
---

**This provides no warranty.**

**First install the dependecies of awesome**

    $ sudo apt-get build-dep awesome

**Tip to have apps versions well organized**

Now install packages xstow, and autoconf for managing manually installed software. (Install git-core if building from git version.)

    $ apt-get install git-core xstow autoconf

Setup xstow dir structure.

    $ [ ! -d /usr/local/stow ] && sudo mkdir -p /usr/local/stow

Make sure installed files have proper permissions:

    $ umask 022

**Ubuntu Lucid uses old version of Freedesktop software - \*xcb\*.**

You need:

libxcb-1.8, xcb-proto-1.7, xcb-util-0.3.8 and xcb-util-wm-0.3.8

Download \*xcb\* package from [1](http://www.freedesktop.org/wiki/Software).

Install libxcb-1.8, xcb-proto-1.7, xcb-util-0.3.8 and xcb-util-wm-0.3.8, use following action (replace $app with proper name of application as written above) to build and install each app.

    $ cd /tmp/
    $ tar xvzf $app.tar.gz
    $ cd $app
    $ ./configure --prefix=/usr/local/stow/$app
    $ sudo make install clean
    $ cd /usr/local/stow
    $ sudo xstow -v $app

In the final you should have following apps build in /usr/local/stow.

    $ ls /usr/local/stow/
    libxcb-1.8  xcb-proto-1.7  xcb-util-0.3.8  xcb-util-wm-0.3.8

**Building awesome-3.4.11 or git version**

Download awesome-3.4.11.tar.bz2 or clone git repo for awesome (if using git replace 3.4.11 with yours date).

    $ cd /tmp
    $ tar xvjf awesome-3.4.11.tar.bz2
    $ cd awesome-3.4.11
    $ cmake -DCMAKE_INSTALL_PREFIX=/usr/local/stow/awesome-3.4.11
    $ make
    $ sudo make install clean
    $ cd /usr/local/stow
    $ sudo xstow -v awesome-3.4.11

Now you should see in /usr/local/stow. (If using git there would be date instead of version.)

    $ ls
    awesome-3.4.11  libxcb-1.8  xcb-proto-1.7  xcb-util-0.3.8  xcb-util-wm-0.3.8

**Post build/install tips**

Check you $PATH if it contains /usr/local/bin.

    $ echo $PATH | grep -q /usr/local/bin ; echo $? # 0 = successfull
    0

**Rebuild cache for libraries**

    $ sudo ldconfig -v
    $ ldconfig -p | egrep "/usr/local/lib/.*xcb" # if you see your new libs in /usr/local it is OK

**Config tips**

I recommend to backup your old rc.lua and move it to somewhere else. Let's start with new version's rc.lua first.

    $ [ -d $HOME/.config/awesome ] && mv $HOME/.config/awesome $HOME/.config/awesome.backup
    $ mkdir -p $HOME/.config/awesome

    $ find /usr/local/ -type f -name 'rc.lua'
    /usr/local/stow/awesome-3.4.11/etc/xdg/awesome/rc.lua
    $ cp /usr/local/stow/awesome-3.4.11/etc/xdg/awesome/rc.lua $HOME/.config/awesome

During awesome building paths to awesome is hardcored, so replace it to be ready for new version.

    $ perl -i -pe 's#stow/awesome-[^/]*/##;' $HOME/.config/awesome/rc.lua
    $ grep 'beautiful.init' $HOME/.config/awesome/rc.lua # just check if paths are OK
    beautiful.init("/usr/local/share/awesome/themes/default/theme.lua")

Start now your new fresh awesome, if it works try to merge your old config with example coming from your new version.

And the proof and little propaganda :)

[<File:Screenshot-small.jpg>](/File:Screenshot-small.jpg "wikilink")