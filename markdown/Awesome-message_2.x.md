---
title: Awesome-message 2.x
permalink: /Awesome-message_2.x/
---

**This is the man page of awesome-client version 2.3.4, compatible with all versions 2.3.x. It can also be used for a big part with previous versions 2.0.x and 2.1.x, but you may read their own man pages for some differences they have.**

<H1>
AWESOME-MESSAGE

</H1>
Section: (1)

<H2>
NAME

</H2>
awesome-message - awesome message window

<H2>
SYNOPSIS

</H2>
awesome-message \[-x xcoord\] \[-y ycoord\] \[-d delay\] <message> <icon>

<H2>
DESCRIPTION

</H2>
awesome-message is a tool which will pop up a simple X window with a message and optionally an icon in front of it.

<H2>
USAGE

</H2>
To have a popup when you have new mail, you can do something like that:

<DL COMPACT>
<DT>
<DD>
    awesome-message "You have new mails!" ~/.awesome/icons/newmail.png

</DL>
<H2>
OPTIONS

</H2>
-x xcoord

<DL COMPACT>
<DT>
<DD>
Set x coordinate of the window.

</DL>
-y ycoord

<DL COMPACT>
<DT>
<DD>
Set y coordinate of the window.

</DL>
-d delay

<DL COMPACT>
<DT>
<DD>
Close the window after <delay> seconds. Must be an integer greater than 0.

</DL>
<message>

<DL COMPACT>
<DT>
<DD>
Print this message in the window.

</DL>
<icon>

<DL COMPACT>
<DT>
<DD>
Draw this icon file in the window.

</DL>
<H2>
SEE ALSO

</H2>
awesome(1)

<H2>
AUTHORS

</H2>
Julien Danjou &lt;julien@danjou.info\[1\]&gt;

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

</DL>
[Category:Awesome2](/Category:Awesome2 "wikilink")