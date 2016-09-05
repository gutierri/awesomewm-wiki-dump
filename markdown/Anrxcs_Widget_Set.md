---
title: Anrxcs Widget Set
permalink: /Anrxcs_Widget_Set/
---

-   This is a small collection of <i>custom</i> functions to be used with [wicked](http://awesome.naquadah.org/wiki/Wicked).

<!-- -->

-   All the <b><i>beautiful.\*.widget</i></b> references can be replaced with custom colors (i.e. <i>"\#ffffff"</i> for white), or include them in your theme file.

<!-- -->

-   Where data parsing is required **only awk is used**, so there are no "cat+head+tail+sed+awk" combinations

<!-- -->

    -- {{{
    --
    -- All functions are licensed under the Creative Commons Attribution-Share Alike License.
    -- To view a copy of this license, visit http://creativecommons.org/licenses/by-sa/3.0/

    -- Battery percentage and state indicator
    --   - example output +95% or -95% when discharging
    mybatwidget = widget({ type = "textbox", name = "mybatwidget", align = "right" })
    function get_batstate()
        local filedescriptor = io.popen('acpitool -b | awk \'{sub(/discharging,/,"-")sub(/charging,|charged,/,"+")sub(/\\./," "); print $4 substr($5,1,3)}\'')
        local value = filedescriptor:read()
        filedescriptor:close()
        return {value}
    end
    wicked.register(mybatwidget, get_batstate, "$1%", 60)


    -- Mail widget
    --   - displays subject of the last e-mail using nail/mailx (heirloom implementation)
    mymailwidget = widget({ type = "textbox", name = "mymailwidget", align = "right" })
    function get_mailsubject()
        local filedescriptor = io.popen('mailx -H -f ~/mail/Inbox | awk \'{ field = $NF }; END{sub(/%/,""); print $10,$11,$12,$13}\'')
        local value = filedescriptor:read()
        filedescriptor:close()
        return {value}
    end
    wicked.register(mymailwidget, get_mailsubject, "$1", 60)


    -- Volume level
    --   - textual volume level of the PCM channel
    myvolwidget    = widget({ type = "textbox", name = "myvolwidget", align = "right" })
    function get_volstate()
        local filedescriptor = io.popen('amixer get PCM | awk \'{ field = $NF }; END{sub(/%/," "); print substr($5,2,3)}\'')
        local value = filedescriptor:read()
        filedescriptor:close()
        return {value}
    end
    wicked.register(myvolwidget, get_volstate, "$1%", 2)


    -- CPU temperature
    mycputempwidget  = widget({ type = "textbox", name = "mycputempwidget", align = "right" })
    function get_temp()
        local filedescriptor = io.popen('awk \'{print $2 "°C"}\' /proc/acpi/thermal_zone/TZS0/temperature')
        local value = filedescriptor:read()
        filedescriptor:close()
        return {value}
    end
    wicked.register(mycputempwidget, get_temp, "$1", 60)


    -- Date, time and a simple calendar launcher
    mydatewidget = widget({ type = "textbox", name = "mydatewidget", align = "right" })
    wicked.register(mydatewidget, wicked.widgets.date, "%b %e, %R", 1)
    mydatewidget:buttons(awful.util.table.join(
        awful.button({ }, 1, function () awful.util.spawn_with_shell("cal -m | xmessage -geometry +1135+17 -file -") end)
    ))

    -- }}}

[Category:Awesome3](/Category:Awesome3 "wikilink") [Category:Widgets](/Category:Widgets "wikilink")