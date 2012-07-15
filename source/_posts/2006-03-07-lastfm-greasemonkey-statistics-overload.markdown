---
date: '2006-03-07 20:49:00'
layout: post
slug: lastfm-greasemonkey-statistics-overload
status: publish
title: Last.fm + Greasemonkey = Statistics Overload
wordpress_id: '68'
categories:
- Code
- Software
---

[![Last.fm GM Scripts In Action](http://static.flickr.com/38/109460996_12e240678b_o.png)](http://www.flickr.com/photos/third/109460996/)

[Last.fm](http://last.fm/), what would I be without you?  A man with a lot more hard drive space, that's for sure.  The amount of music I have come across because of this amazing social service is staggering.  The official iTunes count is 13837 items, 35.8 days, 56.23GB.  And, thanks to Last.fm, [I can track how much of that I actually listen to and enjoy](http://last.fm/user/tupton/).

That's the thing.  I can see my charts, and I can see my top artists.  It's awesome.   It's not as awesome as some people think it could be, though.  With the help of the increasingly awesome / useful Firefox extension [Greasemonkey](http://greasemonkey.mozdev.org/), I now have access to so many music and listening stats that I could spend hours looking at them... Not that I do that.  The image at the top of the post show's my [father's profile](http://www.last.fm/user/aupton/) with all the userscripty goodness attached.  Here are the Last.fm Greasemonkey scripts that I use.

[Last.fm Prettifier](http://thomas.fiveuptons.com/gm/) ([Right-click to install](http://www.fiveuptons.com/thomas/gm/lastfmprettifier.user.js))
First up, we have my own concoction.  This script changes the links in Last.fm charts to block elements so that they take up the whole row and makes them Audioscrobbler Red (#D20039) on hover.  It's a simple change, but it's effective, especially with a wide screen.

[Last.fm Percentages](http://userscripts.org/scripts/show/2365)
This displays a percentage next to the number in all Last.fm charts.  It's useful to see how big a chunk a certain band can take up in terms of your listening habits.  I changed the color to a more neutral grey.  My wish for this script:  Make the weekly percentages out of the total number of songs played that week, not the total number of songs played in the profile.  Also, if there was a way to extend the chart rows to always accomodate this new information, that'd be great.  Sometimes the percentages bleed over into the white of the page when the chart is too narrow.  Shouldn't there be a way to fix this?

[Last.fm User Statistics](http://userscripts.org/scripts/show/2252)
This script inserts a daily / weekly / monthly average of tracks played beneath your total stats on the left side.  I modified this script to change the format of the data.  I'm averaging 68.9 tracks a day after seven months of tracking.  That includes the iPod tracks, too.  I have a life.

[Highlight Same Artists](http://userscripts.org/scripts/show/3009)
This awesome script highlights Top 50 artists on other profiles that you share with that particular user.  I changed the CSS to make the highlighted artists AS red and  bold so they stand out a lot more.

[Last.fm Chart Changes](http://userscripts.org/scripts/show/1982)
This absolutely incredible script tracks (via a [server-side script](http://lastfm.indiegigs.co.uk/)) changes to your charts from week to week.  The relative position to last week is shown to the left of the artist, and appropriate notifications are made for new and stagnant artists.  This script is unbelievable.  Good work, [Mr. Wilkinson](http://indiegigs.co.uk/).
