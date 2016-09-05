---
title: Awesome-menu 2.x
permalink: /Awesome-menu_2.x/
---

**This is the man page of awesome-client version 2.3.4, compatible with all versions 2.3.x. It can also be used for a big part with previous versions 2.0.x and 2.1.x, but you may read their own man pages for some differences they have.**

<H1>
AWESOME-MENU

</H1>
Section: (1)

<H2>
NAME

</H2>
awesome-menu - awesome menu system

<H2>
SYNOPSIS

</H2>
awesome-menu \[-c config\] \[-e command\]

<title>
<H2>
DESCRIPTION

</H2>
awesome-menu is a tool which will pop up a menu on your screen, grabbing keyboard and allowing you to search through an initial completion list, or using your files as completion.

<H2>
USAGE

</H2>
By default, awesome-menu reads completion from standard input. If nothing is read, the completion list is built from the current working directory files.

To build a menu with all the executable files of /usr/bin:

<DL COMPACT>
<DT>
<DD>
    ls /usr/bin | awesome-menu -e 'exec ' 'Execute'

</DL>
To build the same menu and run the programs in a terminal window:

<DL COMPACT>
<DT>
<DD>
    ls /usr/bin | awesome-menu -e 'xterm -e exec ' 'Execute in terminal'

</DL>
To build an ssh connection prompt:

<DL COMPACT>
<DT>
<DD>
    cut -d' ' -f1 ~/.ssh/known_hosts | cut -d, -f1 | awesome-menu -e 'xterm -e ssh ' 'ssh to:'

</DL>
If you do not specify the -e option, the result will be sent to standard output. You can do things like that:

<DL COMPACT>
<DT>
<DD>
    gzip "$(awesome-menu '"File to gzip')"

</DL>
<H2>
OPTIONS

</H2>
-c config

<DL COMPACT>
<DT>
<DD>
Use an alternate configuration file instead of $HOME/.awesomerc.

</DL>
-e command

<DL COMPACT>
<DT>
<DD>
Command to execute. The result is appended to the end of this command (like xargs).

</DL>
<title>
<DL COMPACT>
<DT>
<DD>
Print this title. This is also used to identify the menu section in the awesomerc file.

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