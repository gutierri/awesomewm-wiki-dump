---
title: Automounting
permalink: /Automounting/
---

In popular desktop environment (Gnome, KDE, XFCE), automounting is installed automatically. You just have to plug-in your USB key to access all your data in Nautilus, D3lphin or Thunar. Since **awesome** is not a desktop environment but a window manager, this feature is not provided by default.

To remain to this, there is a standalone application (without any dependencies) that does automounting very well, you can check below solutions. ivman does the job pretty well, and was nice for not having dependencies, but seems dead since 2007. halevt seems to be its successor.

Halevt
======

Ivman project seems to be dead (the last release was in 2007), and it does not work with latest versions of hal and dbus. Its successor is halevt [1](http://www.nongnu.org/halevt/). The configuration file for halevt has very much in common with ivman.

Debian users may install halevt from testing branch with:

    # aptitude install halevt

Also, /etc/PolicyKit/PolicyKit.conf should contain something like

    <match user="halevt">
        <match action="org.freedesktop.hal.storage.mount-removable">
            <return result="yes"/>
        </match>
    </match>

for things to work.

ivman
=====

On a Debian-based distribution, you can install this software by typing in a terminal:

`  sudo apt-get install ivman`

On Archlinux, you can install this software from AUR:

`  `[`http://aur.archlinux.org/packages.php?ID=18938`](http://aur.archlinux.org/packages.php?ID=18938)

On FreeBSD, you can install this software by typing in a terminal:

`  su root -c pkg_add\ -r\ ivman`

On Gentoo Linux:

`  # emerge sys-apps/ivman`
`  # /etc/init.d/ivman start`

Or add it to runlevel:

`  # rc-update -a ivman default`

Not for Gentoo Linux: After it is installed, simply add [Ivman](http://ivman.sourceforge.net/) to your *~/.xinitrc*. Here's an example of what it may look like:

`  #!/bin/sh`
`  ivman &`
`  exec awesome`

Or, simply start ivman and enjoy automounting.

Udisks and udisks-glue
======================

I think this is the better solution. It uses [udisks](http://hal.freedesktop.org/releases/) and [udisks-glue](http://github.com/fernandotcl/udisks-glue). Its configuration file syntax is simple, if you've used awesome 2.x, you'll like it (libconfuse-base ;)).

example of udisks-glue configuration file: this configuration generate popup each time udisks-glue mount or unmount a device:

` filter disks {`
`          optical = false`
`          partition_table = false`
`          usage = filesystem`
` }`
` match disks {`
`          automount = true`
`          automount_options = sync`
`          post_mount_command = "echo \'naughty.notify({title = \"USB:\", text =\"mounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
`          post_unmount_command = "echo \'naughty.notify({title = \"USB:\", text =\"unmounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
` }`
` filter optical {`
`         optical = true`
` }`
` match optical {`
`         automount = true`
`         automount_options = ro`
`         post_mount_command = "echo \'naughty.notify({title = \"CD-Rom:\", text =\"mounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
`         post_mount_command = "echo \'udisks_glue:mount_device(\"%device_file\",\"%mount_point\",\"Cdrom\")\' | awesome-client"`
`         post_unmount_command = "echo \'naughty.notify({title = \"CD-Rom:\", text =\"unmounted %device_file on %mount_point\", timeout = 10})\' | awesome-client"`
` }`

See also the [module blingbling for a menu based on the udisks-glue events](/Blingbling#example_of_udisks-glue_menu: "wikilink").

Autofs
======

Good ol' peace of software that allows to make mounting and unmounting some devices automagic. Only drawback is obscure mount point names. If someone knows a more universal solution using automount, please contribute. The root configuration file is auto.master, which is read at start by the autofs management script. The daemon run by autofs script - automount - must run at all times for the automounting to actually work.

Autofs mounts devices on demand, e.g. when trying to auto-complete the directory name or just typing in the full path to a known file on the device and trying to access it. Autofs is able to unmount devices when they are not used, after a delay. From man autofs: For each of those mount points (in config file) automount(8) will mount and start a thread, with the appropriate parameters, to manage the mount point.

Debian:

    # aptitude install autofs

Some options trough /etc/default/autofs. E.g. i changed the location of auto.master to /etc/autofs/auto.master as i like autofs to have a separate directory. Script resides in /etc/init.d/autofs

Arch Linux:

    # pacman -Sy autofs

Root configuration file is /etc/autofs/auto.master by default. Script in /etc/rc.d/autofs

Config. Read the manual aswell ;)

auto.master

    +auto.master
    #As far as i know, any number of files from any location can be sourced from here.
    #automount anything described in auto.auto at /media, unmounting after 2 idle seconds.
    /media  /etc/autofs/auto.auto --timeout 2

auto.auto

    #linux      -ro,soft,intr       ftp.example.org:/pub/linux
    # all sd devices with the directory names as 2 last chars (b1, c1, e.g. /media/b1 for /dev/sdb1)
    *   -fstype=auto,async,nodev,nosuid,gid=100,uid=1000       :/dev/sd&
    #mounts devices/partitions with known names
    devname   -fstype=auto,async,nodev,nosuid,gid=100,uid=1000       LABEL="DEVNAME"
    my   -fstype=auto,async,nodev,nosuid,gid=100,uid=1000       LABEL="My Passport"
    # mount a single device
    cd  -fstype=auto,ro,nodev,nosuid :/dev/cdrom

As seen from examples, autofs can also automount network shares and the like.

udev and uam
============

udev
----

udev can be configured to automount. The main advantage of this is that if you are already running udev it does not require additional daemons. However the unmount command can't be used by non-root users to unmount things mounted this way. One option is to set it up use synchronous transfers so that it is safe to remove the device as long as you let saves finish, and to unmount things after they are removed; this may shorten the lifespan of some flash memory devices, though as far as I know this is not a serious issue for most uses of most current flash devices. The other option is to use pmount instead of mount. One place to find examples of how to set this up is [the udev page on ArchWiki](https://wiki.archlinux.org/index.php/Udev#Auto_mounting_USB_devices).

uam
---

[uam](https://wiki.archlinux.org/index.php/Udev#Auto_mounting_USB_devices) will automatically set up udev to provide you with nicely automounted things.

On gentoo:

`  # emerge uam pmount`
`  # usermod -aG plugdev username-you-want-to-be-able-to-use-automounted-things`
`  # /etc/init.d/udev restart`

Automounted things will appear as /media/$LABEL, and can be unmounted with `uam-pumount` `/media/$LABEL`.

Udiskie
=======

Another fine solution, using [udisks](http://hal.freedesktop.org/releases/) is a python script - [udiskie](http://comapt.freecode.com/projects/udiskie). This tool works in background and mounts new media automatically. You can use **udiskie-mount** and **udiskie-umount** tools to explicitly mount and unmount partitions and devices.

Additionally, if you have PyGTK installed and want a system tray icon to handle mounts, launch udiskie the following way:
**udiskie --icon**

    awful.util.spawn("udiskie --tray")