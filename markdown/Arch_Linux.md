---
title: Arch Linux
permalink: /Arch_Linux/
---

How to install awesome
======================

Awesome 3
---------

You can install the latest stable version from the official repository:

    # pacman -S awesome

Awesome-git
-----------

There is a PKGBUILD for awesome-git in the AUR. There are several ways to install it.

The easiest is to use "yaourt" (also available in the AUR). If you have yaourt installed, simply:

    # yaourt -S awesome-git

Or you can do it manually, and get the PKGBUILD [here](http://aur.archlinux.org/packages/aw/awesome-git/PKGBUILD).

Create a new directory called "awesome-git" somewhere, put the PKGBUILD in there, and run:

    # makepkg -fi

**Note:** the -i flag automatically installs it aswell, to just create the package without installing first, remove that flag.

How to start awesome
====================

Setup xinitrc
-------------

Setup ~/.xinitrc file, add line for start awesome

    exec awesome

Setup for Slim
--------------

1) Edit slim.conf for start awesome session, add awesome to sessions line

    sessions               awesome,wmii,xmonad

2) Edit ~/.xinitrc file

    DEFAULT_SESSION=awesome
    case $1 in
      awesome) exec awesome ;;
      wmii) exec wmii ;;
      xmonad) exec xmonad ;;
      *) exec $DEFAULT_SESSION ;;
    esac

[Category:Awesome3](/Category:Awesome3 "wikilink")