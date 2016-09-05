---
title: Awesome-3-Gentoo-Portage
permalink: /Awesome-3-Gentoo-Portage/
---

If you would prefer the more recent and less tested version:

`# echo x11-wm/awesome >> /etc/portage/package.keywords`

Or if you want to install a specific unstable version but don't want to automatically update to newer unstable versions (replace 3.4.6 with the version you want; see available versions at the [gentoo package site](http://packages.gentoo.org/package/x11-wm/awesome).)

`# echo "=x11-wm/awesome-3.4.6" >> /etc/portage/package.keywords"`

If you want awesome-client to work and don't have the dbus use flag in /etc/make.conf:

`# echo "x11-wm/awesome dbus" >> /etc/portage/package.use`

Then emerge:

`# emerge -a awesome`

[Category:Awesome3](/Category:Awesome3 "wikilink")