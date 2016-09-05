---
title: Asmer imap mail widget
permalink: /Asmer_imap_mail_widget/
---

[Image:indicator_email_dmj.png](/Image:indicator_email_dmj.png "wikilink")

This widget shows count of new mail in your mailbox, checked via python script by IMAP

Python script
=============

    #!/usr/bin/python

    import imaplib

    #first field is imap server, second - port (993 for gmail SSL IMAP)
    M=imaplib.IMAP4_SSL("imap.gmail.com", 993)
    #first field is imap login (gmail uses login with domain and '@' character), second - password
    M.login("user@gmail.com","password")

    status, counts = M.status("Inbox","(MESSAGES UNSEEN)")

    unread = counts[0].split()[4][:-1]
    if int(unread) == 0:
        print "  0  "
    else:
        print "<span color='red'>  <b>"+unread+"</b>  </span>"

    M.logout()

Save this script in ~/scripts/unread.py and set executable permission to it.

rc.lua configuration
====================

    --- Mail updater
    mymail = widget({ type = "textbox", align = "right" })
    mymail.text = "  ?  "

Hook (check mail every 30 seconds)

    awful.hooks.timer.register(30, function ()
        local f = io.open("/home/YOUR_USER/tmp/gmail")
        local l = nil
        if f ~= nil then
           l = f:read() -- read output of command
        else
           l = "  ?  "
        end
        f:close()

        mymail.text = l
        os.execute("~/scripts/unread.py > ~/tmp/gmail &")
    end)

------------------------------------------------------------------------

Porting to 3.5
==============

<b>~/scripts/unread.py</b>:

    #!/usr/bin/python

    ## change YOUR* pseudo-variables according to your needs

    import imaplib

    #default imap port is 993, change otherwise
    M=imaplib.IMAP4_SSL("YOUR.IMAP.SERVER", 993)
    M.login("YOUR_MAIL","YOUR_PASSWORD")

    status, counts = M.status("Inbox","(MESSAGES UNSEEN)")

    unread = counts[0].split()[4][:-1]

    print(int(unread))

    M.logout()

in <b>rc.lua</b>:

    -- My mail updater widget
    function mailcount()
        os.execute("~/scripts/unread.py > ~/.mailcount")
        local f = io.open(home .. "/.mailcount")
        local l = nil
        if f ~= nil then
              l = f:read()
        else
              l = "?"
        end
        f:close()
        return l
    end

    mymail = wibox.widget.textbox( mailcount() )
    mymail.timer = timer{timeout=60}
    mymail.timer:connect_signal("timeout", function () mymail:set_text ( mailcount() ) end)

where

    home = os.getenv("HOME")

--[Luke Bonham](/User:Luke_Bonham "wikilink") 18:40, 17 February 2013 (CET)

[Category:Awesome3](/Category:Awesome3 "wikilink")