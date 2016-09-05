---
title: Awesome-3-Ubuntu-git
permalink: /Awesome-3-Ubuntu-git/
---

**THESE INSTRUCTIONS NO LONGER WORK WITH GIT -- ONLY AWESOME 3.0**
------------------------------------------------------------------

SKIP GIT STEP AND DOWNLOAD 3.0 AT <http://awesome.naquadah.org/download>
------------------------------------------------------------------------

This is based on the [Ubuntu_Hardy](/Ubuntu_Hardy "wikilink") guide and you can follow up this to both, build the last release and the git HEAD version. Enjoy ;-)

1) Make sure your system is up-to-date, then install the GNU Toolchain, the git VCS and some debian/ubuntu packages.

    sudo apt-get install build-essential autoconf automake libtool gperf xmlto dpatch fakeroot git git-core debhelper

2) Next, install the X.org development packages required to build awesome.

    sudo apt-get install libx11-dev libxinerama-dev libxrandr-dev libpango1.0-dev libimlib2-dev libgtk2.0-dev libxcb-shm0-dev libxcb-render0-dev libxcb-randr0-dev libxcb-shape0-dev libcairo2-dev libxcb-xinerama0-dev liblua5.1-filesystem0 liblua5.1-logging libdirectfb-dev libxt-dev libx11-xcb-dev

3) Optionally you should get asciidoc (to build documentations and developer reference)

    sudo apt-get install asciidoc

4) Now get the packages related with Lua language:

    sudo apt-get install lua5.1 liblua5.1-0-dev

5) Get cmake from Ubuntu Intrepid (unreleased at the time of writing)

<http://packages.ubuntu.com/intrepid/i386/cmake/download> or

<http://packages.ubuntu.com/intrepid/amd64/cmake/download> And install it

    sudo dpkg -i cmake*.deb

6) Get libev from Ubuntu Intrepid (unreleased at the time of writing)

<http://packages.ubuntu.com/intrepid/i386/libev3/download> or

<http://packages.ubuntu.com/intrepid/amd64/libev3/download>

<http://packages.ubuntu.com/intrepid/i386/libev-dev/download> or

<http://packages.ubuntu.com/intrepid/amd64/libev-dev/download> And install it

    sudo dpkg -i libev*.deb

7) Get luadoc from Ubuntu Intrepid (unreleased at the time of writing)

<http://packages.ubuntu.com/intrepid/all/luadoc/download>

<http://packages.ubuntu.com/intrepid/all/liblua5.1-doc0/download>

And install it

    sudo dpkg -i *lua*doc*.deb

8) Build dependencies

get xcb-util:

    git clone git://anongit.freedesktop.org/git/xcb/util
    cd util && ./autogen.sh && make && sudo make install

**Note:** If you are following these instructions under Debian it is not necessary to build libcairo2-dev (It does, at least, work with version 1.6.4-6.1 from the Lenny repos)

    apt-get source libcairo2-dev
    cd cairo-1.6.0
    sed -i.orig -e '/dh_shlibdeps/s/^/#/;s/--disable-xcb/--enable-xcb/' debian/rules
    sudo dpkg-buildpackage -rfakeroot
    sudo dpkg -i ../libcairo2_1.6.0-0ubuntu2_i386.deb ../libcairo2-dev_1.6.0-0ubuntu2_i386.deb
    cd ../..

**Note:** Do not allow libcairo2 and libcairo2-dev to be updated by any package managers. They will be reverted to repo versions, breaking awesome.

9) [Download awesome](http://awesome.naquadah.org/download/) and build and install it.

    git clone git://git.naquadah.org/awesome.git
    cd awesome && make && sudo make install
    <if cmake complains that something is missing, apt-cache search for it and add it to this wiki as well>

**Note:** If the build stops with an 'error stating path', simply create the directory and start again :

    /usr/bin/lua5.1: /usr/share/lua/5.1/luadoc/taglet/standard.lua:447: error stating path `/path/to/awesome/.build-bill-i486-linux-gnu-4.2.3/luadoc'
    stack traceback:
    [...]

    mkdir /path/to/awesome/.build-bill-i486-linux-gnu-4.2.3/luadoc
    make

If this doesn't work (my error was slightly different):

    rm -rf /path/to/awesome/.build-bill-i486-linux-gnu-4.2.3/luadoc
    mkdir /path/to/awesome/.build-bill-i486-linux-gnu-4.2.3/luadoc
    make

10) Create an ~/.xinitrc file and link it to ~/.Xsession

Create ~/.xinitrc with the following contents:

    #!/usr/bin/env bash
    xsetroot -solid black &
    exec /usr/local/bin/awesome

Create a link to ~/.xinitrc and link it to ~/.Xsession:

    ln -s ~/.xinitrc ~/.Xsession

11) Now, when you're in the login screen (aka gdm), select 'Sessions', and switch to 'Xsession' from the list. This will run your .xinitrc script.

GNOME and awesome
-----------------

Chances are, however, you still want to use some parts of GNOME with awesome. You can do this! I have the following entries in my ~/.xinitrc:

    gnome-screensaver &
    gnome-settings-daemon &
    gnome-power-manager &
    nm-applet &

Note: when editing your .xinitrc, always make sure 'exec awesome' is last.

[Category:Awesome3](/Category:Awesome3 "wikilink")