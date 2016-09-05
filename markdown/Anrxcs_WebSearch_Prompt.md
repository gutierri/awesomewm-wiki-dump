---
title: Anrxcs WebSearch Prompt
permalink: /Anrxcs_WebSearch_Prompt/
---

Description
-----------

This is a web search prompt utilizing the YubNub.org (social) command line for the web. YubNub enables you to submit your search to just about any search form on the web thus giving you access to the whole web from only one run prompt. Let's explain how it works:

-   Web search: <b>awesome window manager</b>
    -   will search google.com for "awesome window manager" because google is the default search engine
    -   Web search: <b>g awesome window manager</b>
        -   will do the same because 'g' is the command for google
-   Web search: <b>gim awesome window manager</b>
    -   will search google images for "awesome window manager"
-   Web search: <b>apr awesome</b>
    -   will search Arch Linux repositories for "awesome"
-   Web search: <b>aur awesome</b>
    -   will search Arch Linux user repository for 'awesome'
-   Web search: <b>wp awesome window manager</b>
    -   will search Wikipedia for "awesome window manager"
-   ...and so on, you get the point.

There are already thousands of commands available, to learn them or even define your own commands visit <http://yubnub.org>
Just to be clear, there is no need to do any data or command parsing in your rc.lua, YubNub takes care of all that, you just need to know the correct command for your search.

Code below will submit your search to YubNub trough Firefox and then automatically switch to the tag where Firefox is - in the current code there is no auto-discovery, but the Firefox tag is hardcoded, in my case that is tag <b>3</b>.

Keybinding and function code
----------------------------

    -- Prompt menus
    -- ...
    -- ...
    -- ...
        awful.key({ altkey }, "F12", function ()
            awful.prompt.run({ prompt = "Web search: " }, mypromptbox[mouse.screen].widget,
                function (command)
                    awful.util.spawn("firefox 'http://yubnub.org/parser/parse?command="..command.."'", false)
                    -- Switch to the web tag, where Firefox is, in this case tag 3
                    if tags[mouse.screen][3] then awful.tag.viewonly(tags[mouse.screen][3]) end
                end)
        end),

[Category:Awesome3](/Category:Awesome3 "wikilink")