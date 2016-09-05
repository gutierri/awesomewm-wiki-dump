---
title: Anrxcs OrgMode Widget
permalink: /Anrxcs_OrgMode_Widget/
---

-   This widget for Emacs org-mode is based on the org-awesome module - copyright of Damien Leone.

<!-- -->

-   This is a cheap hack on the org-awesome module to work with [wicked](http://awesome.naquadah.org/wiki/Wicked). For most purposes you are better of with the [original](http://dleone.fensalir.fr/index.php?tag/org-awesome).

<!-- -->

-   Excerpt from rc.lua:

<!-- -->

    --
    -- Agenda and Todo (Emacs org-mode)
    --   * Derived from the org-awesome module, copyright of Damien Leone
    --   * Licensed under the terms of the GNU General Public License version 2
    --     as published by the Free Software Foundation.

    myorgwidget = widget({ type = "textbox", name = "myorgwidget", align = "right" })

    function get_agenda()
       local agenda_files = {
           os.getenv("HOME") .. "/.org/work.org",
           os.getenv("HOME") .. "/.org/index.org",
           os.getenv("HOME") .. "/.org/personal.org"
       }
       local today  = os.time{year=os.date("%Y"), month=os.date("%m"), day=os.date("%d")}
       local soon   = today+24*3600*3 -- 3 days ahead is close
       local future = today+24*3600*7 -- 7 days ahead max
       local count  = { past = 0, today = 0, soon = 0, future = 0 }

       for i = 1, #agenda_files do
          local filedescriptor = io.open(agenda_files[i], "r")
          for line in filedescriptor:lines() do
             local scheduled = string.find(line, "SCHEDULED:")
             local closed    = string.find(line, "CLOSED:")
             local deadline  = string.find(line, "DEADLINE:")
             if (scheduled and not closed) or (deadline and not closed) then
                local b, e, y, m, d = string.find(line, "(%d%d%d%d)-(%d%d)-(%d%d)")
                if b then
                   local  t  = os.time{year=y, month=m, day=d}
                   if     t  < today  then count.past   = count.past   + 1
                   elseif t == today  then count.today  = count.today  + 1
                   elseif t <= soon   then count.soon   = count.soon   + 1
                   elseif t <= future then count.future = count.future + 1
                   end
                end
             end
          end
          filedescriptor:close()
       end
       local value = "$past|$today|$soon|$future"
       value = string.gsub(value, "$past",   "<span color='" .. beautiful.fg_urgent .. "'>"       .. count.past ..   "</span>")
       value = string.gsub(value, "$today",  "<span color='" .. beautiful.fg_normal .. "'>"       .. count.today ..  "</span>")
       value = string.gsub(value, "$soon",   "<span color='" .. beautiful.fg_widget .. "'>"       .. count.soon ..   "</span>")
       value = string.gsub(value, "$future", "<span color='" .. beautiful.fg_netup_widget .. "'>" .. count.future .. "</span>")
       return value
    end

    wicked.register(myorgwidget, get_agenda, "$1", 240)

    myorgwidget:buttons(awful.util.table.join(
        awful.button({ }, 1, function () awful.util.spawn("emacsclient --eval '(org-agenda-list)'", false) end)
    ))

[Category:Awesome3](/Category:Awesome3 "wikilink") [Category:Widgets](/Category:Widgets "wikilink")