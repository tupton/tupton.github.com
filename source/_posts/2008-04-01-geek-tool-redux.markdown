---
date: '2008-04-01 00:11:37'
layout: post
slug: geek-tool-redux
status: publish
title: Geek Tool Redux
wordpress_id: '197'
categories:
- Code
- Howto
- Software
tags:
- bash
- osx
- perl
---

I [last wrote about GeekTool] [geektool article] when I had an iTunes script and some process management output on my desktop. I have a few new scripts that I'll highlight here.

[geektool article]: http://www.thomasupton.com/blog/?p=65


    tail -25 /var/log/system.log

The first script displays the last 25 lines of the system log. In Mac OS X 10.5, the system log rolls over at midnight every day, and this for some reason causes the built-in file display to malfunction. I am using `tail` to display the last 25 lines of the system log.
    
    ps -ecm -O %cpu,%mem

This script shows the output of all currently running processes, sorted by memory usage.

*   `-e` Displays processes from all users.
*   `-c` Only displays the executable name, not the entire path to the excutable.
*   `-m` Sorts the processes by memory usage.
*   `-O` Designates the start of a list of comma-delimited options. `%cpu` displays CPU usage as a percentage. `%mem` displays memory usage as a percentage.
	
    cal | sed "s/^/ /;s/$/ /;s/ $(date +%e) / $(date +%e | sed 's/./-/g') /"

This command outputs a formatted display of the current month. The current date is replaced by the string "--" in order to visually differentiate it.

    /path/to/script/iTunes-Now-Playing.pl | iconv -f utf-8 -t ucs-2-internal

This Perl script is something I wrote to display the currently playing track in iTunes. You can [download the script here] [script]. The script contains a function that was derived from John Gruber's article [How to Determine if a Certain App Is Running Using AppleScript and Perl] [running app].
    
[script]: http://www.thomasupton.com/wp/wp-content/uploads/2008/04/itunes-now-playingpl.zip
[running app]: http://daringfireball.net/2006/10/how_to_tell_if_an_app_is_running
    
    /usr/bin/curl -s http://rss.wunderground.com/auto/rss_full/VA/Blacksburg.xml?units=both | awk '/%3Ctitle%3ECurrent/ {printf "Temp: " $4 " | "; for (i = 8; i %3C 11; i++) if ($i != "-") printf $i " "; else break; printf "\n"}'

**UPDATE:** I now use a [Python script that uses the Yahoo! Weather API](http://www.thomasupton.com/blog/?p=202) to get weather data.
        
This bash script scrapes the [Weather Underground] [wunderground] feed for [Blacksburg] [] and displays the temperature and current conditions. It's not as pretty as a Dashboard weather widget, but it takes up much less screen real estate. I have another instance of this command set up to scrape the Oak Ridge XML feed for when I'm back in Greensboro.
        
[wunderground]: http://www.wunderground.com/
[blacksburg]: http://www.wunderground.com/cgi-bin/findweather/getForecast?query=24060

Each one of these scripts is set up to display for both my 20" Cinema Display and my MacBook screen; I have Home and Portable groups of commands set up in GeekTool. You can [download GeekTool from Tynsoe's website] [geektool].

[geektool]: http://projects.tynsoe.org/en/geektool/
