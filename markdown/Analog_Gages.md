---
title: Analog Gages
permalink: /Analog_Gages/
---

This article provides code for four simple widgets that provide a memory usage indicator, a CPU utilization gage, a user "jiffies" counter, and a simple analog clock. No external scripts are required, just standard Awesome/Lua libraries and the Linux /proc filesystem. All code was tested and verified to work on awesome v3.4 (Closing In).

From left to right: Memory, CPU, jiffies, clock [<File:Analoggages.png>](/File:Analoggages.png "wikilink")

Analog Clock
------------

This creates a little analog clock widget. Simply add the following code to your rc.lua:

`  analogclock = widget({type = "imagebox"})`
`  analogclock.image = image.argb32(24, 24, nil)`
`  function drawclock(ib, d, bg, fg)`
`      ib.image:draw_rectangle(0, 0, d, d, true, bg)`
`      local r = (d - (d % 2))/2`
`      ib.image:draw_circle(r, r, r-1, r-1, false, fg)`
`      local t = os.date("*t")`
`      local ht =  ((t.hour % 12) / 12 + t.min / 720 + t.sec / 43200) * 2 * math.pi`
`      local hx =  math.floor(0.60 * r * math.sin(ht))`
`      local hy = -math.floor(0.60 * r * math.cos(ht))`
`      local mt =  (t.min / 60 + t.sec / 3600) * 2 * math.pi`
`      local mx =  math.floor(0.90 * r * math.sin(mt))`
`      local my = -math.floor(0.90 * r * math.cos(mt))`
`      local st =  t.sec / 60 * 2 * math.pi`
`      local sx =  math.floor(0.90 * r * math.sin(st))`
`      local sy = -math.floor(0.90 * r * math.cos(st))`
`      ib.image:draw_line(r, r, r+sx, r+sy, "#d80000")`
`      ib.image:draw_line(r, r, r+mx, r+my, fg)`
`      ib.image:draw_line(r, r, r+hx, r+hy, fg)`
`      ib.image = ib.image`
`  end`
`  analogtimer = timer { timeout = 1 }`
`  analogtimer:add_signal("timeout", function()`
`      drawclock(analogclock, 24, beautiful.bg_normal, beautiful.fg_normal)`
`  end)`
`  analogtimer:start()`

And add "analogclock" to your status bar:

`   statusbar.widgets = { analogclock }`

Memory Gage
-----------

Adds a memory usage indicator. Paste this code in your rc.lua:

`   function analogmem(ib, d, bg, fg)`
`       local r = (d - (d % 2))/2`
`       ib.image:draw_rectangle(0, 0, d, d, true, bg)`
`       ib.image:draw_circle(r, r, r-1, r-1, false, fg)`
`       ib.image:draw_line(r + math.floor((r - 1) * math.cos(1.25*math.pi)),`
`                    r - math.floor((r - 1) * math.sin(1.25*math.pi)),`
`                    r + math.floor(0.75 * r * math.cos(1.25*math.pi)),`
`                    r - math.floor(0.75 * r * math.sin(1.25*math.pi)), fg)`
`       ib.image:draw_line(r + math.floor((r - 1) * math.cos(1.75*math.pi)),`
`                    r - math.floor((r - 1) * math.sin(1.75*math.pi)),`
`                    r + math.floor(0.75 * r * math.cos(1.75*math.pi)),`
`                    r - math.floor(0.75 * r * math.sin(1.75*math.pi)), fg)`
`       local total    = nil`
`       local active   = nil`
`       for line in io.lines("/proc/meminfo") do`
`           local name, value = string.match(line, "(%w+):\ +(%d+)")`
`           if name == "MemTotal" then`
`               total  = value`
`           else`
`               if name == "Active" then`
`                   active = value`
`               end`
`           end`
`       end`
`       if total and active then`
`           local t =  1.25 * math.pi - (math.pi * 1.5 * active / total)`
`           local x =  math.floor(0.90 * r * math.cos(t))`
`           local y = -math.floor(0.90 * r * math.sin(t))`
`           ib.image:draw_line(r, r, r + x, r + y, "#d80000")`
`       end`
`       ib.image = ib.image`
`   end`
`   meminfo = widget({ type = "imagebox" })`
`   meminfo.image = image.argb32(24, 24, nil)`
`   memtimer = timer { timeout = 1 }`
`   memtimer:add_signal("timeout", function()`
`       analogmem(meminfo, 24, beautiful.bg_normal, beautiful.fg_normal)`
`   end)`
`   memtimer:start()`

And add "meminfo" to your status bar widgets.

CPU Gage
--------

A CPU utilization indicator. Paste this code in your rc.lua:

`   jiffies = {}`
`   function analogcpu(ib, d, bg, fg)`
`       local r = (d - (d % 2))/2`
`       ib.image:draw_rectangle(0, 0, d, d, true, bg)`
`       ib.image:draw_circle(r, r, r-1, r-1, false, fg)`
`       ib.image:draw_line(r + math.floor((r - 1) * math.cos(1.25*math.pi)),`
`                    r - math.floor((r - 1) * math.sin(1.25*math.pi)),`
`                    r + math.floor(0.75 * r * math.cos(1.25*math.pi)),`
`                    r - math.floor(0.75 * r * math.sin(1.25*math.pi)), fg)`
`       ib.image:draw_line(r + math.floor((r - 1) * math.cos(1.75*math.pi)),`
`                    r - math.floor((r - 1) * math.sin(1.75*math.pi)),`
`                    r + math.floor(0.75 * r * math.cos(1.75*math.pi)),`
`                    r - math.floor(0.75 * r * math.sin(1.75*math.pi)), fg)`
`       for line in io.lines("/proc/stat") do`
`           local cpu, newjiffies = string.match(line, "(cpu%d+)\ +(%d+)")`
`           if cpu and newjiffies then`
`               if not jiffies[cpu] then`
`                   jiffies[cpu] = newjiffies`
`               end`
`               local t =  1.25 * math.pi - math.pi * 1.5 * (newjiffies - jiffies[cpu]) / 50`
`               local x =  math.floor(0.90 * r * math.cos(t))`
`               local y = -math.floor(0.90 * r * math.sin(t))`
`               ib.image:draw_line(r, r, r + x, r + y, fg)`
`               jiffies[cpu] = newjiffies`
`           end`
`       end`
`       ib.image = ib.image`
`   end`
`   cpuinfo = widget({ type = "imagebox" })`
`   cpuinfo.image = image.argb32(24, 24, nil)`
`   cputimer = timer { timeout = 0.5 }`
`   cputimer:add_signal("timeout", function()`
`       analogcpu(cpuinfo, 24, beautiful.bg_normal, beautiful.fg_normal)`
`   end)`
`   cputimer:start()`

This code uses the technique described in [CPU Usage](/CPU_Usage "wikilink"). Note that the update/sample rate is important to the calculation.

Again, add "cpuinfo" to your statusbar widgets.

User Jiffies Counter
--------------------

This counts the number of "jiffies" (1/100ths of a second of processor time) dedicated to user processes. Each jiffy spent on a user process will increment the needle for that processor. Paste this code into your rc.lua:

`   function analogjiffies(ib, d, bg, fg)`
`       local r = (d - (d % 2))/2`
`       ib.image:draw_rectangle(0, 0, d, d, true, bg)`
`       ib.image:draw_circle(r, r, r-1, r-1, false, fg)`
`       for line in io.lines("/proc/stat") do`
`           local cpu, jiffies = string.match(line, "(cpu%d*)\ +(%d+)")`
`           if cpu and jiffies then`
`               local t = -jiffies / 100 * 2 * math.pi`
`               local x =  math.floor(0.90 * r * math.cos(t))`
`               local y = -math.floor(0.90 * r * math.sin(t))`
`               if cpu == "cpu" then`
`                   ib.image:draw_line(r, r, r+x, r+y, "#d80000")`
`               else`
`                   ib.image:draw_line(r, r, r+x, r+y, fg)`
`               end`
`           end`
`       end`
`       -- Ridiculous, but necessary:`
`       ib.image = ib.image`
`   end`
`   jiffyinfo = widget({ type = "imagebox" })`
`   jiffyinfo.image = image.argb32(24, 24, nil)`
`   jiffytimer = timer { timeout = 0.05 }`
`   jiffytimer:add_signal("timeout", function()`
`       analogjiffies(jiffyinfo, 24, beautiful.bg_normal, beautiful.fg_normal)`
`   end)`
`   jiffytimer:start()`

And add "jiffyinfo" to your status bar.