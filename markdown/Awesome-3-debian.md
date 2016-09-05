---
title: Awesome-3-debian
permalink: /Awesome-3-debian/
---

-   Get awesome source: `apt-src` `install` `awesome`
-   (Optional) Build without dbus (D-Bus)
    -   Edit `awesomeConfig.cmake`: change `option(WITH_DBUS` `"build` `with` `D-BUS"` `ON)` to `OFF`.
    -   Edit `CMakeLists.txt`: comment out `${SOURCE_DIR}/manpages/awesome-client.1.txt` and `install(FILES` `"utils/awesome-client"` `...`.
    -   Edit `debian/control`: delete dependency on dbus-x11.
-   (Optional) Build with luajit (Tested with awesome 3.4.15)
    -   `apt-get` `install` `libluajit-5.1-dev`
    -   Apply the two commits [1](https://github.com/raedwulf/awesome/commit/38072991b58befcab619309a1251c40e22669367) [2](https://github.com/raedwulf/awesome/commit/564576bcd7be437adc02087202f05a6740ace148) ([see also](https://awesome.naquadah.org/bugs/index.php?do=details&task_id=890)).
-   (Optional) Specify local build version
    -   Edit `debian/changelog`: create a new section with the same format and change the version to something like `3.4.15-1+local.1`
-   `apt-src` `build` `awesome`
-   `dpkg` `-i` `awesome*.deb`

3.4.13 (wheezy)
---------------

*current stable version is [3.4.13-1](http://packages.qa.debian.org/a/awesome.html)*

    # optional step if you want to do debugging
    sudo apt-get install gdb libx11-6-dbg libpango1.0-0-dbg libpango1.0-0-dbg libglib2.0-0-dbg libglib2.0-0-dbg
    # install prerequisites
    sudo apt-get install --no-install-recommends gperf lua5.1 xmlto luadoc libxcb-randr0-dev libxcb-xtest0-dev \
     libxcb-xinerama0-dev  libxcb-shape0-dev libxcb-keysyms1-dev libxcb-icccm4-dev libx11-xcb-dev lua-lgi-dev \
     libstartup-notification0-dev libxdg-basedir-dev libxcb-image0-dev libxcb-util0-dev libimlib2-dev libev-dev
    # download stable source
    apt-get source awesome
    cd awesome-3.4.13
    # build .deb-package
    debuild -us -uc
     # alternatively use dpkg-buildpackage from dpkg-dev if you have no devscripts installed
     # (the difference is that debuid runs lintian after the build.
     # lintian is a package checker which finds packaging errors)
    # install package
    sudo dpkg -i ../awesome_3.4.13-1_i386.deb # or whatever architecture you have

3.5.1
-----

*For installation from git see [Awesome-3-git-debian](/Awesome-3-git-debian "wikilink").*

-   the depency changed:
    -   Drop luadoc, libpango1.0-dev, libev-dev, libimlib2-dev, and gperf.
    -   Push libgdk-pixbuf2.0-dev

<!-- -->

    sudo apt-get install --no-install-recommends lua5.1 xmlto luadoc libxcb-randr0-dev libxcb-xtest0-dev \
     libxcb-xinerama0-dev  libxcb-shape0-dev libxcb-keysyms1-dev libxcb-icccm4-dev libx11-xcb-dev lua-lgi-dev \
     libstartup-notification0-dev libxdg-basedir-dev libxcb-image0-dev libxcb-util0-dev libgdk-pixbuf2.0-dev
    apt-get source awesome
    cd awesome-3.5.1
    debuild -us -uc
    sudo dpkg -i ../awesome_3.5.1-1_i386.deb

Note that you need to [update your configuration file](/Awesome_3.4_to_3.5 "wikilink"), or awesome will fail to start ([\#529642](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=529642)) with something like `attempt` `to` `call` `field` `'add_signal'` `(a` `nil` `value)`. See ~/.xsession-errors if awesome does not start.

If you did not change any config, just use the new version

` cp awesomerc.lua /etc/xdg/awesome/rc.lua`

[Category:Awesome3](/Category:Awesome3 "wikilink")