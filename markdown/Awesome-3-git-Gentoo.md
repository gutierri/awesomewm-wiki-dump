---
title: Awesome-3-git-Gentoo
permalink: /Awesome-3-git-Gentoo/
---

This page is intended to show Gentoo users how to use awesome 3 from [git](http://git.naquadah.org/?p=awesome.git).

*You need to have root access all of this*

I have this script to automatically fetch the latest version of awesome and wicked; it creates symlinks for the man pages and the wicked library. It also fetches the latest version of xcb-util in case you want the latest version.

-   Put the script in /usr/local/src, it doesn't matter what you name it (I named it update.sh)

`#! /bin/sh`
`if [ "$(id -u)" != "0" ]; then`
`echo "This script must be run as root" 1>&2`
`  exit 1`
`fi`
`rm -rvf /usr/local/src/awesome-git`
`git clone `[`git://git.naquadah.org/awesome.git`](git://git.naquadah.org/awesome.git)` /usr/local/src/awesome-git`
`rm -rvf /usr/local/src/xcb-util-git`
`git clone `[`git://anongit.freedesktop.org/git/xcb/util`](git://anongit.freedesktop.org/git/xcb/util)` /usr/local/src/xcb-util-git`
`rm -rvf wicked-git`
`rm -rv /usr/local/share/awesome/lib/wicked.lua`
`rm -rv /usr/share/man/man7/wicked.7.gz`
`git clone `[`git://git.glacicle.com/awesome/wicked.git`](git://git.glacicle.com/awesome/wicked.git)` /usr/local/src/wicked-git`
`ln -sv /usr/local/src/wicked-git/wicked.lua /usr/local/share/awesome/lib/wicked.lua`
`ln -sv /usr/local/src/wicked-git/wicked.7.gz /usr/share/man/man7/wicked.7.gz`

-   Make that executable with

`chmod +x update.sh`

Now you need to edit your /etc/portage/package.keywords (if it isn't there, create it) to allow you to get the versions of cmake (version 2.6&gt;), xcb (version 1.1&gt;), xcb-util (version 0.3.0&gt;), xproto (version 7.0.12&gt;), and xcb-proto (version 1.1&gt;).

-   Your /etc/portage/package.keywords should look like this; along with anything else you already had in there.

`>=dev-util/cmake-2.6.2 ~x86`
`>=x11-libs/libxcb-1.1 ~x86`
`>=x11-libs/xcb-util-0.3.0 ~x86`
`>=x11-proto/xproto-7.0.12 ~x86`
`>=x11-proto/xcb-proto-1.1 ~x86`

Replace ~x86 with your arch (~amd64, et cetera).

Now all you have to do is emerge the dependencies for awesome, compile awesome, and enjoy!

To emerge the dependencies, just type this command in at your terminal (as root)

`emerge -uav app-doc/doxygen dev-util/cmake dev-util/luadoc dev-lang/lua x11-libs/cairo x11-libs/libxcb x11-libs/pango x11-libs/xcb-util`
`x11-proto/xproto`

That should be all of the dependencies, if you get an error during the awesome compile, figure it out (you *are* using Gentoo, you should be competent enough to do that ;)).

Now to compile awesome, you need git to fetch the latest source for awesome. Just use the update script and it will do all of the fetching for you.

Once you have the source, go into the awesome-git folder, and compile and install awesome.

-   Here's how you compile awesome

`cd /usr/local/src/awesome-git`
`make`
`make install`

Now just add exec awesome to your .xinitrc and start X!

[Category:Awesome3](/Category:Awesome3 "wikilink")