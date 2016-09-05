---
title: Awesome-client 3.x
permalink: /Awesome-client_3.x/
---

**This is the man page of awesome-client version 3.4.1. It can also be used for a large part with previous versions 3.x, because there are not much differences, apart the communication mode with awesome (since 3.3). Nevertheless, you may read their specific man pages if it is necessary.**

<H2>
NAME

</H2>
awesome-client - awesome window manager remote execution

<H2>
SYNOPSIS

</H2>
awesome-client

<H2>
DESCRIPTION

</H2>
awesome-client is a remote command line interface to awesome. It communicates with awesome via D-Bus, allowing remote execution of Lua code.

<H2>
USAGE

</H2>
awesome-client reads commands from standard input and sends them via D-Bus to awesome. If <I>rlwrap</I> is installed, it will be used to provide a readline command line interface.

The <I>awful.remote</I> module has to be loaded if you want this command to work.

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

[Category:Awesome3](/Category:Awesome3 "wikilink")