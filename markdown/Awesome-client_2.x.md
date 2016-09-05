---
title: Awesome-client 2.x
permalink: /Awesome-client_2.x/
---

**This is the man page of awesome-client version 2.3.4, compatible with all versions 2.3.x. It can also be used for a big part with previous versions 2.0.x and 2.1.x, but you may read their own man pages for some differences they have.**

<H1>
AWESOME-CLIENT

</H1>
Section: (1)

<H2>
NAME

</H2>
awesome-client - awesome window manager command line interface

<H2>
SYNOPSIS

</H2>
awesome-client

<H2>
DESCRIPTION

</H2>
awesome-client is the command line interface to awesome. It communicates with awesome via a socket located in the users's HOME directory.

<H2>
USAGE

</H2>
To determine which socket is to be used, it reads the DISPLAY environment variable. awesome-client reads commands from standard input.

When you pipe multiple lines into awesome-client, an empty line will flush already collected lines into awesome with an according immediate execution.

The command format is: screen_number command argument

For example, to change a statusbar textbox text on screen 0, you can do the following:

<DL COMPACT>
<DT>
<DD>
    echo 0 widget_tell <statusbar-name> <textbox-name> text Hello, world | awesome-client

</DL>
To change an iconbox image on screen 1, you can do the following:

<DL COMPACT>
<DT>
<DD>
    echo 0 widget_tell <statusbar-name> <iconbox-name> image /home/user/image.jpg | awesome-client

</DL>
To view tag number 3 on screen 1:

<DL COMPACT>
<DT>
<DD>
    echo 1 tag_view 3 | awesome-client

</DL>
To zoom focused window on screen 0:

<DL COMPACT>
<DT>
<DD>
    echo 0 client_zoom | awesome-client

</DL>
<H2>
SEE ALSO

</H2>
awesome(1) awesomerc(5)

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