---
date: '2005-11-09 17:20:00'
layout: post
slug: tab-sidebar
status: publish
title: Tab Sidebar
wordpress_id: '45'
categories:
- Design
- Howto
- Software
tags:
- extension
- firefox
---

While browsing the day's [popular delicious links](http://del.icio.us/popular/), I came across a [Firefox](http://www.mozilla.org/products/firefox/) extension that I had never seen before.  [Tab Sidebar](http://users.blueprintit.co.uk/~dave/web/firefox/tabsidebar/index.html) uses Firefox's sidebar to give a visual representation of the contents of each of your open tabs.  There are options to reload and close the tab, as well as display the URL of each tab.  It's a pretty nifty extension, but it was obviously made for the Windows version of Firefox.  I made a few images and replaced the existing ones in the extension.

To install these new images, first quit Firefox and then go to


    
    ~/Library/Application Support/Firefox/Profiles/[profile name]/extensions/



In this folder should be a list of all your extensions.  Most extensions are named very cryptic-like, but fortunately for us, Tab Sidebar has named itself properly.  Open the folder


    
    TabSidbar@blueprint.co.uk/chrome/skin/



BACK UP ALL THE IMAGE FILES IN THIS FOLDER!

I made a new folder called Original Images, and put all the gifs and tiffs in there.

Next, unzip [the new images](http://www.thomasupton.com/wp/wp-content/uploads/2008/04/tab_sidebar_mac_widgets.zip) into this folder.  Restart Firefox, et voilà!  Tab Sidebar has been Aquafied!  I also chose to assign Cmd-Shift-T to the Tab Sidebar menu item (do this under the Keyboard And Mouse preference pane in System Preferences), but I don't think Firefox likes that too much.  I'll keep playing around with that to see if I can get it to work.

[![Tab Sidebar, Aquafied](http://static.flickr.com/31/61688338_91ea2f7954_o.png)](http://www.flickr.com/photos/third/61688338/)

Tab Sidebar requires Firefox 1.5b1 or higher.  I take no responsibility for messing up your installation of Firefox or Tab Sidebar.
