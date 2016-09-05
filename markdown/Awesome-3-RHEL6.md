---
title: Awesome-3-RHEL6
permalink: /Awesome-3-RHEL6/
---

Steps to build and install Awesome 3 on RHEL6 Beta

Prerequisite
------------

-   Install *Development tools* and *Desktop Platform Development*.

` yum groupinstall -y 'Development tools' 'Desktop Platform Development'`

-   Install *ImageMagick*, *startup-notification* and *readline*.

` yum install -y ImageMagick startup-notification startup-notification-devel readline readline-devel`

-   Set PKG_CONFIG_PATH to include */usr/local/lib/pkgconfig/*. Note, if you open a new terminal/session, don't forget to set this variable.

` export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"`

-   Get [cmake](http://www.cmake.org/), build and install it. *Tested with version 2.8.1*.

` ./bootstrap && make && make install`

-   Get [gperf](http://www.gnu.org/software/gperf/), build and install it. *Tested with version 3.0.4*.

` ./configure && make && make install`

-   Get [imlib2](http://sourceforge.net/projects/enlightenment/files/imlib2-src), build and install it. *Tested with version 1.4.4*.

` ./configure && make && make install`

-   Get [libev](http://software.schmorp.de/pkg/libev.html), build and install it. *Tested with version 3.9*.

` ./configure && make && make install`

-   Get [libpthread-stubs xcb-proto libxcb xcb-util xcb-util-renderutil xcb-util-image xcb-util-cursor](http://xcb.freedesktop.org/dist/), build and install in this order. *Tested with version 0.3 of libpthread-stubs, 1.6 of libxcb, 1.6 of xcb-proto, 0.3.6 of xcb-util, 0.3.9 of xcb-util-renderutil, 0.4.0 of xcb-util-image and 0.1.2 of xcb-util-cursor*.
-   For all

` ./configure && make && make install`

-   Get [libxdg-basedir](http://n.ethz.ch/~nevillm/download/libxdg-basedir/), build and install it. *Tested with version 1.1.0*.

` ./configure && make && make install`

-   Get [Lua](http://www.lua.org/), build and install it. *Tested with version 5.1.4*.

` make linux && make install`

-   Get [Cairo](http://cairographics.org/), build and install it. *Tested with version 1.8.10*.

` ./configure --enable-xcb && make && make install`

Awesome
-------

-   Get [Awesome](http://awesome.naquadah.org/download/), build and install it. *Tested with version 3.4.5*.

` make && make install`

After Installation
------------------

-   Awesome is installed in */usr/local/*.
-   You need a configuration file for awesome in your home (*~/.config/awesome/rc.lua*). The default is in */usr/local/etc/xdg/awesome/*.
-   To run Awesome you have to add */usr/local/lib* to */etc/ld.so.conf* and run *ldconfig*, otherwise Awesome is not able to find some libraries.

Notes
-----

-   Documentation of Awesome isn't available. To build you need *xmlto*, *asciidoc* and *luadoc*.
-   I tried to rebuild cairo with xcb enabled from SRC-RPM, but no luck I got a error about missing xcb. Furthermore a update of the cairo package from Red Hat will break the awesome build.
