---
title: Awesome-3-Slackware
permalink: /Awesome-3-Slackware/
---

To build Awesome 3 on Slackware, you have to recompile Cairo because the version that ships with Slackware does not include xcb support. You also have to install Lua, Imlib2, xcb-util and libev. Let's see how to do this step by step, under Slackware 13:

CMake
-----

I had to upgrade to 2.8 to make it work. If you need to upgrade, download the source, slackbuild, run it and then upgrade the package with upgradepkg command.

Cairo
-----

Apply this [patch](http://bitbucket.org/jfsantos/slackbuilds/raw/874117ed359d/cairo.SlackBuild.patch) to the Slackware's default cairo.SlackBuild (if you do not have the sources disc, retrieve this from a Slackware mirror). After applying this patch and recompiling the package, Cairo will have xcb support. In fact, this patch just adds the flag --enable-xcb when running configure.

Lua and imlib2
--------------

Install Lua and Imlib2 with the [SlackBuilds.org](http://www.slackbuilds.org) SlackBuilds: [SlackBuild for Lua](http://slackbuilds.org/repository/13.0/development/lua/), [SlackBuild for Imlib2](http://slackbuilds.org/repository/13.0/libraries/imlib2/).

xcb-util and libev
------------------

I made SlackBuilds for these packages. Check my SlackBuilds [repository](http://bitbucket.org/jfsantos/slackbuilds/). You can get a copy of all my SlackBuilds with this command: "hg clone <http://www.bitbucket.org/jfsantos/slackbuilds/>". By the way, this repository includes the patch for Cairo, too.

xcb, libstartup-notification, libxdg
------------------------------------

In order to compile version 3.4.4 you need to upgrade these packages as well. For the first two, I grabbed the packages off of -current as they were updated enough to be used. Be warned though, that mixing -stable and -current packages is generally a bad idea. I wanted to test it out, and so far I can see no side effects from using libxcb and libstartup-notification from -current, ymmv. (Note that libxcb is in mirror/x/X11/lib). As for libxdg, I found a slackbuild from vector linux that seemed to work (with some slight edits). [Here it is](http://pastebin.com/WiUH4DEL)

Awesome
-------

I also made a SlackBuild for Awesome. Just build it after installing all the above dependencies and install it. The package built by this SlackBuild also adds Awesome to the xwmconfig menu, so just select it and startx :)

[Category:Awesome3](/Category:Awesome3 "wikilink")