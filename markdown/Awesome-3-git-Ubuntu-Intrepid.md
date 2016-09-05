---
title: Awesome-3-git-Ubuntu-Intrepid
permalink: /Awesome-3-git-Ubuntu-Intrepid/
---

This is based on the [Awesome-3-Ubuntu-git](/Awesome-3-Ubuntu-git "wikilink") guide and you can follow up this to both, build the last release and the git HEAD version. Enjoy ;-)

1) Make sure your system is up-to-date, then install the GNU Toolchain, the git VCS and some debian/ubuntu packages.

    $ sudo aptitude install build-essential autoconf automake libtool gperf xmlto
    $ sudo aptitude install dpatch fakeroot git git-core debhelper

2) Next, install the X.org development packages required to build awesome.

    $ sudo aptitude install libx11-dev libxinerama-dev libxrandr-dev \
            libpango1.0-dev libimlib2-dev libgtk2.0-dev libxcb-shm0-dev \
            libxcb-render0-dev  libxcb-randr0-dev libxcb-shape0-dev \
            libcairo2-dev libxcb-xinerama0-dev liblua5.1-filesystem0 \
            liblua5.1-logging libdirectfb-dev libxt-dev libx11-xcb-dev cmake \
            lua5.1 liblua5.1-0-dev libev3 libev-dev luadoc liblua5.1-doc0 \
            libxcb-aux0 libxcb-keysyms0 libxcb-xtest0-dev

3) Optionally you should get asciidoc (to build documentations and developer reference)

    $ sudo aptitude install asciidoc

4) Build dependencies

get xcb-util:

    $ git clone git://anongit.freedesktop.org/git/xcb/util
    $ cd util && ./autogen.sh && ./configure && make && sudo make install

<strong>Note</strong> as of 19 Jan 2009, awesome-3.1.1 will not build against a HEAD checkout of xcb-utils. See [this mailing list thread](http://www.mail-archive.com/awesome@naquadah.org/msg00438.html) for details; as a workaround (as noted in that thread), download [the xcb-util-0.3.2 tarball](http://xcb.freedesktop.org/dist/xcb-util-0.3.2.tar.bz2), expand it, and do the standard `./configure` `&&` `make` `&&` `sudo` `make` `install`.

5) Build awesome

    $ git clone git://git.naquadah.org/awesome.git
    $ cd awesome && make && sudo make install

6) Create an ~/.xinitrc file and link it to ~/.Xsession

Create ~/.xinitrc with the following contents:

    #!/usr/bin/env bash
    xsetroot -solid black &
    exec /usr/local/bin/awesome

Create a link to ~/.xinitrc and link it to ~/.Xsession:

    ln -s ~/.xinitrc ~/.Xsession

7) Now, when you're in the login screen (aka gdm), select 'Sessions', and switch to 'Xsession' from the list. This will run your .xinitrc script.

GNOME and awesome
-----------------

Chances are, however, you still want to use some parts of GNOME with awesome. You can do this! I have the following entries in my ~/.xinitrc:

    gnome-screensaver &
    gnome-settings-daemon &
    gnome-power-manager &
    nm-applet &

Note: when editing your .xinitrc, always make sure 'exec awesome' is last.

Installation into an alternate location
---------------------------------------

I wanted to be able to install into `/opt/awesome-3.1.1`, which required some modifications to the above. (N.b. this worked for me, but may not be the best/right way to do it; YMMV.)

First, when building xcb-util, specify the installation location during the configure, then build and install as normal:

    ./configure --prefix=/opt/awesome-3.1.1 && make && make install

Second, when building awesome, tell it where to find the xcb-util libraries and specify the installation location:

    sed -i.old 's/usr\/local/opt\/awesome-3.1.1/' awesomeConfig.cmake
    PKG_CONFIG_PATH=/opt/awesome-3.1.1/lib/pkgconfig/ cmake -DPREFIX=/opt/awesome-3.1.1
    make && sudo make install

Note that the above 'make install' still requires root-level access because it drops a file into `/etc/xdg/`. I haven't yet played around with relocating that file, or simply not installing it.

[Category:Awesome3](/Category:Awesome3 "wikilink")