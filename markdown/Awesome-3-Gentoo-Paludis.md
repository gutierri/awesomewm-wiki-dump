---
title: Awesome-3-Gentoo-Paludis
permalink: /Awesome-3-Gentoo-Paludis/
---

Add the following repository to /etc/paludis/repositories into a [.conf](http://paludis.pioto.org/configuration/repositories.html)

[`git://gitorious.org/thewtex-repo/mainline.git`](git://gitorious.org/thewtex-repo/mainline.git)

There are some awesome-3 ebuilds that can be now found in the gentoo portage repository, but they are masked, which will affect these ebuilds too. Therefore, you will need to put *x11-wm/awesome* in your /etc/paludis/package_unmask.conf.

Keyword *x11-wm/awesome* in /etc/paludis/keywords.conf with *~amd64* or *~x86* if you want the latest release or *\** if you want the latest git -scm build.

`paludis --install --pretend awesome`

Keyword like a gangster.

`paludis --install awesome`

[Category:Awesome3](/Category:Awesome3 "wikilink")