---
title: Antel MPD-Ruby
permalink: /Antel_MPD-Ruby/
---

We will create a MPD widget using a MPD client written in Lua (that doesn't use mpc/netcat/telnet).

The MPD Ruby Library
--------------------

Install ruby, then with rubygem install librmpd

Linux-Freebsd: gem i librmpd

MPD Widget
----------

We want to take the output from our app, so we write this wimple widget:

    function get_command_output (command)
        local c = io.popen(command)
        local output = {}
        i = 0
       return c:read("*line")
    end

    mympd = widget({ type = "textbox", name = "mympd", align = "left" })

Remember to add "mympd," to statusbar. To make it working, you need to call it, so you can add it to hook_timer\[every second\] that's included default on rc.lua or create your own:

    function hook_timer ()
        mytextbox.text = " " .. os.date() .. " "
        mympd.text = " " .. get_command_output("ruby /MODIFY/YOUR/PATH/mpdr.rb") .. " "
    end

Then be sure to register the hook:

    [INCLUDED IN DEFAULT RC.LUA]
    awful.hooks.timer.register(1, hook_timer)

Code of mpdr.rb:

    #Ruby MPD wrapper

    require 'rubygems'
    require 'librmpd'

    HOST = "localhost"
    PORT = 6000

    mpd = MPD.new HOST, PORT

    mpd.connect
    #mpd.password('mypassword')
    if mpd.stopped?
        mpd.play
    end
    song = mpd.current_song

    #Time calculation
    time = mpd.status["time"]
    time = time.split(':')
    elapsed = time[0].to_i
    el_min = elapsed / 60
    el_sec = elapsed % 60
    elapsed = "#{el_min}:#{el_sec}"

    total = time[1].to_i
    tot_min = total / 60
    tot_sec = total % 60
    total = "#{tot_min}:#{tot_sec}"

    time = "#{elapsed}/#{total}"


    #Adjusting output of Artist
    artist = "#{song.artist}"
    artist.gsub!(/ & /, '/')

    #Adjusting output of Title
    title = "#{song.title}"
    title.gsub!(/_/, ' ')
    title.gsub!(/ & /, '/')
    title.gsub!(/feat/i, 'Ft')
    title.gsub!(/remix/i, 'rmx')

    #Some hacky to get correct output if id3tags are strange
    if artist.empty?
        puts "[#{song.file} - #{time}]"
    elsif title.empty?
        puts "[#{song.file} - #{time}]"
    elsif artist =~ /artist/i
        puts "[#{song.file} - #{time}]"
    elsif title =~ /track/i
        puts "[#{song.file} - #{time}]"
    else
        puts "[#{artist} - #{title} - #{time}]"
    end

    mpd.disconnect

[Category:awesome3](/Category:awesome3 "wikilink")