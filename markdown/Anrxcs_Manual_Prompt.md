---
title: Anrxcs Manual Prompt
permalink: /Anrxcs_Manual_Prompt/
---

Description
-----------

I use this run prompt to read manual pages all the time. It has fast completion and is often faster to access a page then switching tags or focus to your 'reader'. I put examples for a terminal emulator, KDE help center and GNU Emacs (which runs as a server or daemon (in version 23)). This version searches through your whole MANPATH.

Keybinding and function code
----------------------------

    -- Prompt menus
    -- ...
    -- ...
    -- ...
    awful.key({ modkey }, "F4", function ()
        awful.prompt.run({ prompt = "Manual: " }, mypromptbox[mouse.screen].widget,
        --  Use GNU Emacs for manual page display
        --  function (page) awful.util.spawn("emacsclient --eval '(manual-entry \"'" .. page .. "'\")'", false) end,
        --  Use the KDE Help Center for manual page display
        --  function (page) awful.util.spawn("khelpcenter man:" .. page, false) end,
        --  Use the terminal emulator for manual page display
            function (page) awful.util.spawn("urxvt -e man " .. page, false) end,
            function(cmd, cur_pos, ncomp)
                local pages = {}
                local m = 'IFS=: && find $(manpath||echo "$MANPATH") -type f -printf "%f\n"| cut -d. -f1'
                local c, err = io.popen(m)
                if c then while true do
                    local manpage = c:read("*line")
                    if not manpage then break end
                    if manpage:find("^" .. cmd:sub(1, cur_pos)) then
                        table.insert(pages, manpage)
                    end
                  end
                  c:close()
                else io.stderr:write(err) end
                if #cmd == 0 then return cmd, cur_pos end
                if #pages == 0 then return end
                while ncomp > #pages do ncomp = ncomp - #pages end
                return pages[ncomp], cur_pos
            end)
    end),

[Category:Awesome3](/Category:Awesome3 "wikilink")