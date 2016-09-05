---
title: AuroraUX
permalink: /AuroraUX/
---

The latest port version of awesome for AuroraUX (OpenSolaris) is awesome-2-Current and awesome-3-Current.

Download
--------

Simply download the awesome 'current' src version.

You may need to build extra dep in the README file if not on AuroraUX.

### Versions

-   The 3.1.1 port has an option for DBUS.
-   If you don't know which version you need, take the latest stable (3.1.1).

Building & Installing Awesome
-----------------------------

`export LIBS="-lX11 -lXext"`
`./autogen`
`./configure --prefix=/usr`
`gmake`
`gmake install`

Post Installation Notes
-----------------------

You need to have a configuration file: `$HOME/.config/awesome/rc.lua` Copy the default configuration file with:

`mkdir -p $HOME/.config/awesome`
`cp /usr/local/share/examples/awesome/awesomerc.lua $HOME/.config/awesome/rc.lua`

Cheers,
[Evocallaghan](/User:Evocallaghan "wikilink")

[Awesome2](/Category:Awesome3 "wikilink")