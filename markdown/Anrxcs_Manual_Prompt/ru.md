---
title: Anrxcs Manual Prompt ru
permalink: /Anrxcs_Manual_Prompt/ru/
---

Описание
--------

Я использую строку запуска для чтения man-страниц все время. Она позволяет быстрее завершить или зачастую быстрее получить доступ к странице, а затем переключить теги и/или сфокусироваться на вашем 'reader'. Я прилагаю примеры для терминала, справочного центра KDE и GNU Emacs (запущенного как сервер или демон (в версии 23)). Эта версия позволяет вести поиск по всему MANPATH.

Сочетание клавиш и код
----------------------

Добавьте следующий код в раздел prompt menu вашего rc.lua

    -- Prompt menus
    -- ...
    awful.key({ modkey }, "F4", function ()
        awful.prompt.run({ prompt = "Manual: " }, mypromptbox[mouse.screen].widget,
        --  Использование GNU Emacs для отображения man-страниц
        --  function (page) awful.util.spawn("emacsclient --eval '(manual-entry \"'" .. page .. "'\")'", false) end,
        --  Использование Справочного Центра KDE  для отображения man-страниц
        --  function (page) awful.util.spawn("khelpcenter man:" .. page, false) end,
        --  Использование эмулятора терминал  для отображения man-страниц
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