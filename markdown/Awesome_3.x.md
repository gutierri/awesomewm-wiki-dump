---
title: Awesome 3.x
permalink: /Awesome_3.x/
---

**This is the man page of awesome version 3.4.1. It can also be used for a large part with previous versions 3.x, because there are not much differences ; for example, the commands Mod4 + w (Open main menu) in version 3.3 and Mod4 + n (Minimize client) in 3.4 version have been added. Nevertheless, you may read their specific man pages if it is necessary.**

<H1>
AWESOME

</H1>
<H2>
NAME

</H2>
awesome - awesome window manager

<H2>
SYNOPSIS

</H2>
<B>awesome</B> \[<B>-v</B> | <B>--version</B>\] \[<B>-h</B> | <B>--help</B>\] \[<B>-c</B> | <B>--config</B> <I>FILE</I>\] \[<B>-k</B> | <B>--check</B>\]

<H2>
DESCRIPTION

</H2>
<B>awesome</B> is a window manager for X. It manages windows in different layouts, like floating or tiled. Any layout can be applied dynamically, optimizing the environment for the application in use and the task currently being performed. In a tiled layout, windows are managed in a master and stacking area. The master area contains the windows which currently need the most attention, whereas the stacking area contains all other windows. In a floating layout windows can be resized and moved freely. Dialog windows are always managed as floating, regardless of the layout currently applied. The spiral and dwindle layouts are special cases of the tiled layout where the stacking area is arranged in a spiral for the former or as a rectangular fractal for the later. Windows are grouped by tags in awesome. Each window can be tagged with one or more tags. Selecting certain tags displays all windows with these tags. <B>awesome</B> can contain small wiboxes which can display anything you want: all available tags, the current layout, the title of the visible windows, text, etc.

<H2>
OPTIONS

</H2>
<B>-v</B>, <B>--version</B>

<DL COMPACT>
<DT>
<DD>
Print version information to standard output, then exit.

</DL>
<B>-h</B>, <B>--help</B>

<DL COMPACT>
<DT>
<DD>
Print help information, then exit.

</DL>
<B>-c</B>, <B>--config</B> <I>FILE</I>

<DL COMPACT>
<DT>
<DD>
Use an alternate configuration file instead of <I>$XDG_CONFIG_HOME/awesome/rc.lua</I>.

</DL>
<B>-k</B>, <B>--check</B>

<DL COMPACT>
<DT>
<DD>
Check configuration file syntax.

</DL>
<H2>
DEFAULT MOUSE BINDINGS

</H2>
<H3>
Navigation

</H3>
<B>Button1</B> on tag name

<DL COMPACT>
<DT>
<DD>
View tag.

</DL>
<B>Button4</B>, <B>Button5</B> on tag name

<DL COMPACT>
<DT>
<DD>
Switch to previous or next tag.

</DL>
<B>Button4</B>, <B>Button5</B> on root window

<DL COMPACT>
<DT>
<DD>
Switch to previous or next tag.

</DL>
<B>Button1</B>, <B>Button3</B>, <B>Button4</B>, <B>Button5</B> on layout symbol

<DL COMPACT>
<DT>
<DD>
Switch to previous or next layout.

</DL>
<H3>
Layout modification

</H3>
<B>Mod4 + Button1</B> on tag name

<DL COMPACT>
<DT>
<DD>
Tag current client with this tag only.

</DL>
<B>Mod4 + Button3</B> on tag name

<DL COMPACT>
<DT>
<DD>
Toggle this tag for client.

</DL>
<B>Button3</B> on tag name

<DL COMPACT>
<DT>
<DD>
Add this tag to current view.

</DL>
<B>Mod4 + Button1</B> on client window

<DL COMPACT>
<DT>
<DD>
Move window.

</DL>
<B>Mod4 + Button3</B> on client window

<DL COMPACT>
<DT>
<DD>
Resize window.

</DL>
<H2>
DEFAULT KEY BINDINGS

</H2>
<H3>
Window manager control

</H3>
<B>Mod4 + Control + r</B>

<DL COMPACT>
<DT>
<DD>
Restart <B>awesome</B>.

</DL>
<B>Mod4 + Shift + q</B>

<DL COMPACT>
<DT>
<DD>
Quit <B>awesome</B>.

</DL>
<B>Mod4 + r</B>

<DL COMPACT>
<DT>
<DD>
Run prompt.

</DL>
<B>Mod4 + x</B>

<DL COMPACT>
<DT>
<DD>
Run Lua code prompt.

</DL>
<B>Mod4 + Return</B>

<DL COMPACT>
<DT>
<DD>
Spawn terminal emulator.

</DL>
<B>Mod4 + w</B>

<DL COMPACT>
<DT>
<DD>
Open main menu.

</DL>
<H3>
Clients

</H3>
<B>Mod4 + Shift + r</B>

<DL COMPACT>
<DT>
<DD>
Redraw the focused window.

</DL>
<B>Mod4 + m</B>

<DL COMPACT>
<DT>
<DD>
Maximize client.

</DL>
<B>Mod4 + n</B>

<DL COMPACT>
<DT>
<DD>
Minimize client.

</DL>
<B>Mod4 + f</B>

<DL COMPACT>
<DT>
<DD>
Set client fullscreen.

</DL>
<B>Mod4 + Shift + c</B>

<DL COMPACT>
<DT>
<DD>
Kill focused client.

</DL>
<B>Mod4 + t</B>

<DL COMPACT>
<DT>
<DD>
Mark a client.

</DL>
<H3>
Navigation

</H3>
<B>Mod4 + j</B>

<DL COMPACT>
<DT>
<DD>
Focus next client.

</DL>
<B>Mod4 + k</B>

<DL COMPACT>
<DT>
<DD>
Focus previous client.

</DL>
<B>Mod4 + u</B>

<DL COMPACT>
<DT>
<DD>
Focus first urgent client.

</DL>
<B>Mod4 + Left</B>

<DL COMPACT>
<DT>
<DD>
View previous tag.

</DL>
<B>Mod4 + Right</B>

<DL COMPACT>
<DT>
<DD>
View next tag.

</DL>
<B>Mod4 + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Switch to tag 1-9.

</DL>
<B>Mod4 + Control + j</B>

<DL COMPACT>
<DT>
<DD>
Focus next screen.

</DL>
<B>Mod4 + Control + k</B>

<DL COMPACT>
<DT>
<DD>
Focus previous screen.

</DL>
<B>Mod4 + Escape</B>

<DL COMPACT>
<DT>
<DD>
Focus previously selected tag set.

</DL>
<H3>
Layout modification

</H3>
<B>Mod4 + Shift + j</B>

<DL COMPACT>
<DT>
<DD>
Switch client with next client.

</DL>
<B>Mod4 + Shift + k</B>

<DL COMPACT>
<DT>
<DD>
Switch client with previous client.

</DL>
<B>Mod4 + o</B>

<DL COMPACT>
<DT>
<DD>
Send client to next screen.

</DL>
<B>Mod4 + h</B>

<DL COMPACT>
<DT>
<DD>
Decrease master width factor by 5%.

</DL>
<B>Mod4 + l</B>

<DL COMPACT>
<DT>
<DD>
Increase master width factor by 5%.

</DL>
<B>Mod4 + Shift + h</B>

<DL COMPACT>
<DT>
<DD>
Increase number of master windows by 1.

</DL>
<B>Mod4 + Shift + l</B>

<DL COMPACT>
<DT>
<DD>
Decrease number of master windows by 1.

</DL>
<B>Mod4 + Control + h</B>

<DL COMPACT>
<DT>
<DD>
Increase number of columns for non-master windows by 1.

</DL>
<B>Mod4 + Control + l</B>

<DL COMPACT>
<DT>
<DD>
Decrease number of columns for non-master windows by 1.

</DL>
<B>Mod4 + space</B>

<DL COMPACT>
<DT>
<DD>
Switch to next layout.

</DL>
<B>Mod4 + Shift + space</B>

<DL COMPACT>
<DT>
<DD>
Switch to previous layout.

</DL>
<B>Mod4 + Control + space</B>

<DL COMPACT>
<DT>
<DD>
Toggle client floating status.

</DL>
<B>Mod4 + Control + Return</B>

<DL COMPACT>
<DT>
<DD>
Swap focused client with master.

</DL>
<B>Mod4 + Control + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Toggle tag view.

</DL>
<B>Mod4 + Shift + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Tag client with tag.

</DL>
<B>Mod4 + Shift + Control + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Toggle tag on client.

</DL>
<B>Mod4 + Shift + F1-9</B>

<DL COMPACT>
<DT>
<DD>
Tag marked clients with tag.

</DL>
<H2>
CUSTOMIZATION

</H2>
<B>awesome</B> is customized by creating a custom <I>$XDG_CONFIG_HOME/awesome/rc.lua</I> file.

<H2>
SIGNALS

</H2>
<B>awesome</B> can be restarted by sending it a SIGHUP.

<H2>
SEE ALSO

</H2>
<B>awesomerc</B>(5) <B>awesome-client</B>(1)

<H2>
BUGS

</H2>
Of course there's no bug in <B>awesome</B>. But there may be unexpected behaviors.

<H2>
AUTHORS

</H2>
Julien Danjou &lt;julien@danjou.info\[1\]&gt; and others.

<H2>
WWW

</H2>
<I><http://awesome.naquadah.org></I>

<H2>
NOTES

</H2>
<DL COMPACT>
<DT>
1. julien@danjou.info

<DL COMPACT>
<DT>
<DD>
[mailto:julien@danjou.info](mailto:julien@danjou.info)

[Category:Awesome3](/Category:Awesome3 "wikilink")