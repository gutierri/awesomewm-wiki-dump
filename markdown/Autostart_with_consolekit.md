---
title: Autostart with consolekit
permalink: /Autostart_with_consolekit/
---

If you want to use the awesome menu to let your computer restart/shutdown/hibernate/suspend, You should do this:

gentoo:

-   You MUST emerge with use +polkit +pam :

<!-- -->

    ➜  ~ git:(master) ✗ epv dbus dbus-glib pambase shadow  upower polkit awesome consolekit
    zsh: correct 'dbus' to '.dbus' [nyae]? n


    These are the packages that would be merged, in order:

    Calculating dependencies... done!
    [ebuild   R    ] sys-apps/dbus-1.4.20  USE="X -debug -doc (-selinux) -static-libs -test" 0 kB
    [ebuild   R    ] dev-libs/dbus-glib-0.98  USE="-debug -doc -static-libs -test" 0 kB
    [ebuild   R    ] x11-wm/awesome-3.4.11  USE="dbus -doc" 0 kB
    [ebuild   R    ] sys-auth/polkit-0.104-r1  USE="gtk introspection nls pam -debug -doc -examples -kde (-selinux) (-systemd)" 0 kB
    [ebuild   R    ] sys-auth/consolekit-0.4.5_p20120320  USE="acl pam policykit -debug -doc (-selinux) -test" 0 kB
    [ebuild   R    ] sys-power/upower-0.9.16  USE="introspection -debug -doc -ios" 0 kB
    [ebuild   R    ] sys-auth/pambase-20101024-r2  USE="consolekit cracklib sha512 -debug -gnome-keyring -minimal -mktemp -pam_krb5 -pam_ssh -passwdqc (-selinux)" 0 kB
    [ebuild   R    ] sys-apps/shadow-4.1.4.3  USE="cracklib nls pam -audit (-selinux) -skey" 1,762 kB

    Total: 8 packages (8 reinstalls), Size of downloads: 1,762 kB

-   kernel MUST enable:

<!-- -->

    ➜  ~ git:(master) ✗  grep audit -i  /usr/src/linux/.config
    CONFIG_AUDIT_ARCH=y
    CONFIG_AUDIT=y
    CONFIG_AUDITSYSCALL=y
    CONFIG_AUDIT_WATCH=y
    CONFIG_AUDIT_TREE=y

-   .xinitrc MUST have:

<!-- -->

    exec ck-launch-session dbus-launch --sh-syntax --exit-with-session awesome >> ~/.awesome_stdout 2>> ~/.awesome_stderr

-   dbus, consolekit MUST start:

<!-- -->

    ➜  ~ git:(master) ✗ rc-update

               consolekit |      default
                     dbus |      default

-   following the ARCH bash style do the auto login, here is my zsh config:

<!-- -->

    ➜  ~ git:(master) ✗ cat /etc/inittab
    #
    # /etc/inittab:  This file describes how the INIT process should set up
    #                the system in a certain run-level.
    #
    # Author:  Miquel van Smoorenburg, <miquels@cistron.nl>
    # Modified by:  Patrick J. Volkerding, <volkerdi@ftp.cdrom.com>
    # Modified by:  Daniel Robbins, <drobbins@gentoo.org>
    # Modified by:  Martin Schlemmer, <azarah@gentoo.org>
    # Modified by:  Mike Frysinger, <vapier@gentoo.org>
    # Modified by:  Robin H. Johnson, <robbat2@gentoo.org>
    #
    # $Header: /var/cvsroot/gentoo-x86/sys-apps/sysvinit/files/inittab-2.87,v 1.1 2010/01/08 16:55:07 williamh Exp $

    # Default runlevel.
    id:3:initdefault:

    # System initialization, mount local filesystems, etc.
    si::sysinit:/sbin/rc sysinit

    # Further system initialization, brings up the boot runlevel.
    rc::bootwait:/sbin/rc boot

    l0:0:wait:/sbin/rc shutdown
    l0s:0:wait:/sbin/halt -dhp
    l1:1:wait:/sbin/rc single
    l2:2:wait:/sbin/rc nonetwork
    l3:3:wait:/sbin/rc default
    l4:4:wait:/sbin/rc default
    l5:5:wait:/sbin/rc default
    l6:6:wait:/sbin/rc reboot
    l6r:6:wait:/sbin/reboot -dk
    #z6:6:respawn:/sbin/sulogin

    # new-style single-user
    su0:S:wait:/sbin/rc single
    su1:S:wait:/sbin/sulogin

    # TERMINALS
    #c1:12345:respawn:/sbin/agetty 38400 tty1 linux
    c1:5:respawn:/sbin/agetty  -a jinleileiking -8 -s 38400 tty1 linux
    c2:2345:respawn:/sbin/agetty 38400 tty2 linux
    c3:2345:respawn:/sbin/agetty 38400 tty3 linux
    c4:2345:respawn:/sbin/agetty 38400 tty4 linux
    c5:2345:respawn:/sbin/agetty 38400 tty5 linux
    c6:2345:respawn:/sbin/agetty 38400 tty6 linux

    # SERIAL CONSOLES
    #s0:12345:respawn:/sbin/agetty 9600 ttyS0 vt100
    #s1:12345:respawn:/sbin/agetty 9600 ttyS1 vt100

    # What to do at the "Three Finger Salute".
    ca:12345:ctrlaltdel:/sbin/shutdown -r now

    # Used by /etc/init.d/xdm to control DM startup.
    # Read the comments in /etc/init.d/xdm for more
    # info. Do NOT remove, as this will start nothing
    # extra at boot if /etc/init.d/xdm is not added
    # to the "default" runlevel.
    x:a:once:/etc/X11/startDM.sh



    ➜  ~ git:(master) ✗ cat .zprofile

    # New environment setting added by Sourcery CodeBench Lite for ARM EABI on Thu Jul 05 17:04:19 CST 2012 1.
    # The unmodified version of this file is saved in /home/jinleileiking/.zprofile2131715756.
    # Do NOT modify these lines; they are used to uninstall.
    PATH="/home/jinleileiking/crosstools/CodeSourcery/Sourcery_CodeBench_Lite_for_ARM_EABI/bin:${PATH}"
    export PATH
    # End comments by InstallAnywhere on Thu Jul 05 17:04:19 CST 2012 1.


    if [[/_-z_$DISPLAY_| -z $DISPLAY ]] && [[/_$(tty)_=_/dev/tty1_| $(tty) = /dev/tty1 ]]; then
      #exec startx -- vt01
      exec startx
      # Could use xinit instead of startx
      #exec xinit -- /usr/bin/X -nolisten tcp vt7
    fi

-   Modify your rc.lua:

<!-- -->

    local upower = [[dbus-send --print-reply \
    --system \
    --dest=org.freedesktop.UPower \
    /org/freedesktop/UPower \
    org.freedesktop.UPower.]]
    local consolkit = [[dbus-send --print-reply \
    --system \
    --dest="org.freedesktop.ConsoleKit" \
    /org/freedesktop/ConsoleKit/Manager \
    org.freedesktop.ConsoleKit.Manager.]]

    mymainmenu = awful.menu({ items = { { "awesome", myawesomemenu, beautiful.awesome_icon },
                                        { "terminal", terminal },
                                        { "chromium", shell_cmd .. "chromium"},
                                        { "lock", "slock" },
                                        { "Suspend", function() awful.util.spawn(upower.."Suspend") end },
                                        { "Hibernate", function () awful.util.spawn(upower.."Hibernate") end },
                                        { "Restart", consolkit.."Restart", icon_path.."restart.png" },
                                        { "Shutdown", consolkit.."Stop", icon_path.."poweroff.png" },
                                      }
                            })