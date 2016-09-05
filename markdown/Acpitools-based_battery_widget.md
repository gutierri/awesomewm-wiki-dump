---
title: Acpitools-based battery widget
permalink: /Acpitools-based_battery_widget/
---

And here's my personal battery widget. It's using [acpitool](http://freeunix.dyndns.org:8000/site2/acpitool.shtml) as a battery information source (so install that first), and uses <span> colors for an added touch (you might need to change the specific colors for contrastual reasons). Other than that, it's heavily based on [Battery Widget using powersave](/Battery_Widget_using_powersave "wikilink").

    mybattmon = widget({ type = "textbox", name = "mybattmon", align = "right" })
    function battery_status ()
        local output={} --output buffer
        local fd=io.popen("acpitool -b", "r") --list present batteries
        local line=fd:read()
        while line do --there might be several batteries.
            local battery_num = string.match(line, "Battery \#(%d+)")
            local battery_load = string.match(line, " (%d*\.%d+)%%")
            local time_rem = string.match(line, "(%d+\:%d+)\:%d+")
        local discharging
        if string.match(line, "discharging")=="discharging" then --discharging: always red
            discharging="<span color=\"#CC7777\">"
        elseif tonumber(battery_load)>85 then --almost charged
            discharging="<span color=\"#77CC77\">"
        else --charging
            discharging="<span color=\"#CCCC77\">"
        end
            if battery_num and battery_load and time_rem then
                table.insert(output,discharging.."BAT#"..battery_num.." "..battery_load.."%% "..time_rem.."</span>")
            elseif battery_num and battery_load then --remaining time unavailable
                table.insert(output,discharging.."BAT#"..battery_num.." "..battery_load.."%%</span>")
            end --even more data unavailable: we might be getting an unexpected output format, so let's just skip this line.
            line=fd:read() --read next line
        end
        return table.concat(output," ") --FIXME: better separation for several batteries. maybe a pipe?
    end
    mybattmon.text = " " .. battery_status() .. " "
    my_battmon_timer=timer({timeout=30})
    my_battmon_timer:add_signal("timeout", function()
        --mytextbox.text = " " .. os.date() .. " "
        mybattmon.text = " " .. battery_status() .. " "
    end)
    my_battmon_timer:start()

Don't forget to register mybattmon.

------------------------------------------------------------------------

Here's a version for acpitool using bashets and tested in Awesome 3.5

` bashets = require("bashets")`
` batterystatus = wibox.widget.textbox()`
` bashets.register("/usr/bin/acpitool -b | cut -d: -f2-", `
`                  {`
`                      widget = batterystatus,`
`                      update_time = 60, `
`                      separator = '|',`
`                      format = "  Battery: $1" `
`                  })`

Then just make sure that:

` right_layout:add(batterystatus) `

has been added to your mywibox configuration and you're ready to go.

Simple and it updates once a minute.

- lowkey

[Category:Awesome3](/Category:Awesome3 "wikilink") [Category:Widgets](/Category:Widgets "wikilink")