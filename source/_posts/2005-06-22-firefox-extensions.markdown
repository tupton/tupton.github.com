---
date: '2005-06-22 23:24:00'
layout: post
slug: firefox-extensions
status: publish
title: Firefox Extensions
wordpress_id: '28'
categories:
- Software
tags:
- extension
- firefox
- list
---

[![](http://photos17.flickr.com/21027787_7e6090a61d.jpg)](http://www.flickr.com/photos/third/21027787/)



	
  * [Adblock](http://adblock.mozdev.org/) - It blocks ads.  Really well.  I use [this site](http://www.pierceive.com/filtersetg/) for all my filters.  Instructions are in the directory at the top.  Apparently, there is a new [Adblock Plus](http://bene.sitesled.com/adblock.htm) in the works, but as of right now, it is not part of the official builds. The new version allows whitelisting of sites, but can anyone tell me when you _want_ ads to be shown while browsing?

	
  * [Automarks](http://extensionroom.mozdev.org/more-info/automarks) - This nifty extension copies your bookmarks to Autocomplete so that when you start typing in the location bar, your bookmarks show up in the drop down menu. The only gripe I have is that you have to tell it to copy them yourself, meaning that after clearing your History you need to Automark again. Surely it could be made to update your Autocomplete history every few minutes?

	
  * [Bug Me Not](http://roachfiend.com/archives/2005/02/07/bugmenot/) - This amazing extension allows you to right click a login form on a registration-required site (such as [nytimes.com](http://nytimes.com)) and use an anonymous login to bypass said compulsory site registration.  Astounding.

	
  * [Disable Targets For Download](http://www.cusser.net/extensions/disabletarget/) - Ever notice that a new window ([ or tab, for trendy about:config geeks](http://kb.mozillazine.org/Firefox_:_FAQs_:_About:config_Entries#Browser..2A)) opens when you click a file to download?  This extension stops those new windows, complete with a user-defined list of file extensions to use.

	
  * [Feedview](http://www.epigoon.com/?page=feedview) - Why Firefox does not do this by default I do not know, but this extension displays RSS XML files with a pretty stylesheet.  I know that Sage (see below) makes this superfluous, but this makes Firefox handle RSS more like Safari does.

	
  * [FLST](http://gorgias.de/mfe/) (scroll down to the bottom) - Focus Last Selected Tab does exactly that.  When you close a tab, Firefox will make active the last tab that had focus, as opposed to making active the next tab in line.

	
  * [Greasemonkey](http://greasemonkey.mozdev.org/) - This extension is probably one of the most flexible and versatile extensions out there.  Greasemonkey allows you to modify sites' content through javascript.  [This wiki](http://dunck.us/collab/GreaseMonkeyUserScripts) has a lot of useful scripts to get you started.

	
  * [LastTab](https://addons.mozilla.org/extensions/moreinfo.php?id=112) - Similar to FLST, this extension modifies the behavior of Ctrl-Tab to focus the last selected tab.

	
  * [Sage](http://sage.mozdev.org/) - This is my most used extension.  Sage is an RSS aggregator that resides in Firefox's sidebar.  Click a button to discover feeds on the current site, add, and read.  I use [Jon Hicks'](http://www.hicksdesign.co.uk/) [Sage modifications for OS X](http://www.hicksdesign.co.uk/journal/sage-on-os-x), even on my PC.  For Windows I get rid of the Mac throbber by commenting out the following code from userChrome.css:
`#rssStatusImage[loading="true"] {
list-style-image: url("sage/checking.gif") !important;
}`

	
  * [Show Failed URL](http://www.pikey.me.uk/mozilla/) (third from the bottom) - By default, Firefox does not show the URL of a page that failed to load; instead it pops up a rather nasty dialog box.  Before you install this extension, type about:config into the location bar, and then type browser.xul into the filter box.  There should only be one option visible; change it to true.  Now, when a page does not load in Firefox, the URL remains in the location bar and there are no more popup dialogs to click through!

	
  * [Sort Extensions](https://addons.mozilla.org/extensions/moreinfo.php?application=firefox&id=460) - Pretty self explanatory.  You can see the changes it makes to the extensions window in the image above.

	
  * [undoclosetab](http://extensionroom.mozdev.org/more-info/undoclosetab) - This extension keeps a history of up to three closed tabs.  Middle click the tab bar, or right click and choose through the context menu to enable.  Unfortunately, it does not work between browser sessions, so if you close a tab, then close the window, it will not remember your last closed tabs.

	
  * [Copy Image](http://extensionroom.mozdev.org/more-info/copyimage) - It adds Copy Image To Clipboard to the context menu of an image.  Very useful, but why is it not built in to Firefox?  I seem to remember enabling this in about:config, but I could not find it again.  If anyone knows anything, please let me know._Update_: After looking at my work PC's context menu, I can see that this functionality is built in to Firefox 1.0.4.  However, on my Powerbook at home, it was not available.  I recently had to do a clean install of Firefox, which meant reinstalling all of my extensions, so I am positive that Copy Image was not there.  Interesting that the menu item is "Copy Image To Clipboard" in the extension but just "Copy Image" in the Windows build.  I'll have to check on my Windows laptop at home to see if Copy Image is available.



A lot of my extensions come from [The Extension Room](http://extensionroom.mozdev.org/) and the [Mozilla Add-Ons Page](https://addons.mozilla.org/extensions/). Feel free to browse through those archives and let me know if you find anything interesting.
