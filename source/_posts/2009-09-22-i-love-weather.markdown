---
date: '2009-09-22 22:53:38'
layout: post
slug: i-love-weather
status: publish
title: I Love Weather
wordpress_id: '468'
categories:
- Articles
- Howto
- Software
tags:
- geektool
- image
- script
- weather
---

**UPDATE**: It seems that Yahoo! has changed their weather URL formats, which breaks the script that fetches the weather image. In order to fix it, go to [Yahoo! Weather](http://weather.yahoo.com/) and enter your desired location. Once you have the correct page loaded, replace the URL in the script with this new one. The script should start working again.



There's no question that my post that describes my script to [get the current weather conditions with Python and Yahoo! weather feeds][script] is the most popular post on my site. It drives a huge proportion of the (small amount of) traffic that this site gets, and the post has many more comments than the average post.

[script]: http://www.thomasupton.com/blog/2008/04/yahoo-weather-feeds-with-python/

Many [people][flickr1] [post][flickr2] images of their desktops in their best GeekTool getup, including ones that feature my weather script. [One of these in particular][gato-image] had a lovely image to go along with his weather conditions. The author, [Gato's][gato-flickr], was kind enough to share how he [retrieved and displayed this image][gato-comment] in the comments of the photo post. I love this idea, and tweaked Gato's' method. I thought I'd detail what I did in case anyone else is interested.

![Partly Cloudy](http://farm3.static.flickr.com/2615/3951420535_9a3d34e105_o.png)

[flickr1]: http://www.flickr.com/photos/megatrond/3100049946/
[flickr2]: http://www.flickr.com/photos/donnyboy09/3436467554/
[gato-image]: http://www.flickr.com/photos/mgastongrillo/3858032308/
[gato-flickr]: http://www.flickr.com/photos/mgastongrillo/
[gato-comment]: http://www.flickr.com/photos/mgastongrillo/3858032308/#comment72157622208211629

Gato's' method of retrieving Yahoo!'s weather images involves *two* additional GeekTool entries: one to fetch the image, and one to display the image on the desktop. I knew that there was a better way to do this, and I knew it involved [`launchd`][launchd]. `launchd` is a system-wide service for starting and stopping daemons. It replaced `init`, `cron`, and many other utilities in OS X 10.4. You can find out a lot more on the previous link and on the [Wikipedia page][launchd-wiki].

[launchd]: http://developer.apple.com/macosx/launchd.html
[launchd-wiki]: http://en.wikipedia.org/wiki/Launchd

I had never used `launchd` before, but setting up my own entry was fairly straightforward after I read a couple of the related man pages. In particular, [`launchd.plist`][man-launchdplist] and [`execvp`][man-execvp] were very useful. In order to get started, you need to use the Property List Editor, found in `/Developer/Applications/Utilities/Property List Editor.app`. If you do not have Apple's developer tools installed, you can create an XML file with a `.plist` extension with the same contents.

[man-launchdplist]: x-man-page://launchd.plist
[man-execvp]: x-man-page://execvp

Before we start creating a `launchd` item, we need the script for the item to run. I have provided a bash script version of Gato's' image-grabbing command. I added a fifteen second timeout argument to the initial `curl` call; this prevents long timeouts for those times that you are without a network connection. You will also need to replace the URL at the end of line three of the script with your weather forecast URL. Simply go to the [Yahoo! weather page][yahoo-weather] and put in your zip or location code in order to retrieve your weather page, then paste that page's URL into the script. You can [download the script here][script-dl].

[script-dl]: http://www.thomasupton.com/wp/wp-content/uploads/2009/09/get_weather_image.sh.zip
[yahoo-weather]: http://weather.yahoo.com/

    #! /bin/bash
    
    /usr/bin/curl --connect-timeout 15 --silent "http://weather.yahoo.com/united-states/virginia/blacksburg-2365044/" |
    /usr/bin/grep "forecast-icon" |
    /usr/bin/sed "s/.*background\:url(\'\(.*\)\')\;\ _background.*/\1/" |
    /usr/bin/xargs curl --silent -o /tmp/weather.png

Now we can create the property list that will launch our image-grabbing script. `launchd` works by parsing a particular format of property list file. The man page for [`launchd.plist`][man-launchdplist] is very extensive and contains documentation on the entries that are allowed in a `launchd` item. If that is all a bit too dense, I will walk through the creation of a simple `launchd` item with the bare minimum of properties needed to launch our script.

Open Property List Editor.app and create a new item by clicking **Add Item** in the toolbar. In the **Key** column, type `Label`. The default **Type** is *string*, so type a label identifier in the **Value** column. I used `com.thomasupton.fetchweatherimage`. You can use whatever you like. Next, add another item and this time switch its **Type** to *boolean*. Type `KeepAlive` as the **Key** and don't check the check box to leave its value as `false`. Add another *number* item called `StartInterval` and give it a value of `600`. Finally, add an *array* item called `ProgramArguments`. Add a child to this array (just click the **Add Item** button with the `Program Arguments` node selected) and give it a value of `/bin/bash`. Add a second child and give it a value of `/absolute/path/to/script/get_weather_image.sh`, with the *absolute* path to where you saved the script. I have pasted the XML contents of the file here, and you can [download the property list file here][plist-dl]. You can open the property list with a text editor to edit the contents if you do not have the Property List Editor installed.

[plist-dl]: http://www.thomasupton.com/wp/wp-content/uploads/2009/09/com.thomasupton.fetchweatherimage.plist

    
    
    
    
    	Label
    	com.thomasupton.fetchweatherimage
    	KeepAlive
    	
    	StartInterval
    	600
    	ProgramArguments
    	
    		/bin/bash
    		/Users/thomasupton/code/script/get_weather_image.sh
    	
    
    

What we have just created is a `launchd` item that will only start on demand every 600 seconds and run our script to grab the weather image. Save the property list to `~/Library/LaunchAgents` and give it a name like *com.thomasupton.fetchweatherimage.plist*. Open Terminal.app and type the following command to load our new `launchd` item, replacing the name of the script with the file name that you saved your property list as.

    $ launchctl load ~/Library/LaunchAgents/com.thomasupton.fetchweatherimage.plist

Back in GeekTool, all you need to do is add one new entry. Make it a picture and give it the URL `file:///tmp/weather.png`. *Et voilà*, you have a beautiful image of your weather conditions on your desktop.

I [took a picture of my current desktop][desktop] to show you what it looks like. I gave the weather image an opacity of 60% for a more subtle effect.

[desktop]: http://www.flickr.com/photos/third/3946891446/
![Snow Leopard Desktop](http://farm3.static.flickr.com/2498/3946891446_da67c218a9.jpg)
