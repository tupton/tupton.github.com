---
date: '2006-09-07 00:15:53'
layout: post
slug: pandoracom-mini-player-popup
status: publish
title: Pandora.com Mini Player Popup
wordpress_id: '88'
categories:
- Code
- Music
- Personal
---




**UPDATE:** Pandora has been redesigned and no longer offers an explicit mini player, so this bookmarklet no longer works as intended.






Continuing in the spirit of all things [Pandora-related](http://www.thomasupton.com/blog/index.php?s=pandora), I decided to make a [Pandora](http://pandora.com) mini-player pop-up bookmarklet in between classes today.

I just wrote a little Javascript function to open a window, wrapped it in a function, and threw it into a bookmark.  There's one little annoyance, however: the window opens in the background.  I have no idea how to make windows open in the foreground, so if anyone knows how to make that happen, I'd be grateful if you could let me know.

Without further ado, I give you the Pandora Mini-Player Bookmarklet.
Tested in Firefox 1.5.0.6, Mac OS X 10.4.7.

To use in Firefox:

 * Drag the button below up to your Bookmarks Bar at the top of your Firefox window, below the Address Bar.
 * Click the new bookmark to enjoy the Pandora Mini-Player at any time!

[![Pandora Radio](http://www.thomasupton.com/images/pandora_button.png)](javascript:function pandoraPopUp(){open('http://pandora.com/?cmd=mini','_blank','width=725,height=400,scrollbars=0,location=0');}pandoraPopUp();)
