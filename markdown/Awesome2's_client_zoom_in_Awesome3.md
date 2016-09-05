---
title: Awesome2's client zoom in Awesome3
permalink: /Awesome2's_client_zoom_in_Awesome3/
---

Awesome2's client_zoom would replace the current master with the client that had focus and back again.

I *think* this is missing in Awesome3, so I implemented it in lua:

    keybinding({ modkey }, "Return",
        function ()
            if  client.focus == awful.client.master() then
                awful.client.focus.history.previous()
            end
            client.focus:swap(awful.client.master())
        end):add()

Enjoy.

--[Cciulla](/User:Cciulla "wikilink") 12:41, 11 September 2008 (UTC)

[Category:Awesome3](/Category:Awesome3 "wikilink")