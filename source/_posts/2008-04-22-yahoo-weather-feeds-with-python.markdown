---
date: '2008-04-22 09:52:04'
layout: post
slug: yahoo-weather-feeds-with-python
status: publish
title: Yahoo! Weather Feeds With Python
wordpress_id: '202'
categories:
- Articles
- Code
- Software
- Webapps
tags:
- python
- weather
- yahoo
---

[Download the weather script from GitHub.](http://github.com/tupton/python-yahoo-weather/tree/master)


  






**UPDATE:** Chris Lasher updated my script. It now utilizes [optparse](http://docs.python.org/lib/module-optparse.html) and docstrings. Some of the options have been changed, too, and I have licensed the code under a [by-nc-sa CC license](http://creativecommons.org/licenses/by-nc-sa/3.0/us/).  I have updated this post to reflect these changes. Thanks, Chris!






I believe that everyone likes to know what the weather is like outside. In the good ol' days, people used to stick a hand out of the window and know everything they needed to know about the weather; now we have [myriad] [google weather] [options] [wunderground weather] to [check] [yahoo weather] the [weather] [weather.gov].

[google weather]: http://www.google.com/search?q=weather%3A24060
[wunderground weather]: http://www.wunderground.com/cgi-bin/findweather/getForecast?query=24060
[yahoo weather]: http://weather.yahoo.com/forecast/USVA0068.html
[weather.gov]: http://forecast.weather.gov/MapClick.php?CityName=Blacksburg&state=VA&site=RNK&textField1=37.2327&textField2=-80.4284

A few years ago, I was running [Konfabulator] [] on Windows. The best widget for this application was the weather widget. I had a large weather widget that showed the current weather in the town in which I was currently residing, and some smaller weather widgets allowed me to keep tabs on the weather elsewhere.

[konfabulator]: http://widgets.yahoo.com/

When I got my Powerbook in 2004, I continued to use Konfabulator. With Tiger came [Dashboard] [] in 2005; I promptly started using the weather widgets provided by Dashboard. However, I realized that the requirement that I switch contexts to Dashboard just to quickly check small tidbits of information greatly distracted me from my workflow. Even the ability to display widgets directly on the desktop was no help; the pretty images take up too much screen real estate.

[dashboard]: http://www.apple.com/downloads/dashboard/

As I have [written] [] about in the past, I utilize [Geek Tool] [] on my desktop. One of the scripts I run scrapes the [Weather Underground] [] XML feed for Blacksburg, VA and displays the current conditions. This works fine, but it isn't very extensible. Weather Underground does not appear to have a feed API, so each city's XML feed must be found manually and utilized in the script. Last night I decided that this was unacceptable.

[written]: http://thomas.fiveuptons.com/?p=197
[geek tool]: http://projects.tynsoe.org/en/geektool/
[weather underground]: http://www.wunderground.com/

Yahoo offers a great [RSS-based API for its weather services] [Yahoo Weather API]. Python has a [built-in XML parser] [python xml]. It doesn't take a [rocket surgeon] [] to put those two together. With a little [help from Yahoo's Developer Network] [yahoo python dev], I was able to come up with a more robust script with which the weather can be displayed.

[yahoo weather api]: http://developer.yahoo.com/weather/
[python xml]: http://docs.python.org/lib/module-xml.dom.minidom.html
[rocket surgeon]: http://omnisio.com/startupschool08/david-heinemeier-hansson-at-startup-school-08
[yahoo python dev]: http://developer.yahoo.com/python/python-xml.html

I learned a few things regarding Python while writing this script. The [getopt() method] [getopt] is a very useful command-line options and arguments parser. It's very simple to define options for your script using this method.

[getopt]: http://docs.python.org/lib/module-getopt.html

    import getopt
    import sys

    opts, args = getopt(sys.argv[1:], "cflv")

Now the options that were passed on the command line and that are part of `cflv` are in the `opts` array; command line arguments are listed in `args`.



**UPDATE:** My script now uses [optparse](http://docs.python.org/lib/module-optparse.html) instead of getopt.



[minidom] [] is a simple but complete DOM implementation. Using the `getElementsByTagName` and the `getAttribute` methods makes for easy and stress-free XML parsing.

[minidom]: http://docs.python.org/lib/module-xml.dom.minidom.html

    from xml.dom.minidom import parse

    dom = parse("file.xml")

    attrs = []

    for node in dom.getElementsByTagName('tag'):
        attrs.append(node.getAttribute('text'))

`attrs` contains the `text` attribute of each `tag` node in the XML file `file.xml`.

I am currently looking into building this script on top of a Twitter model. Keep checking in for more information.

In order to run the script, pass it a zip code to find the weather.

    $ weather.py 24060

Running the script without any arguments will show the usage and the available options.


* `-c` Suppress the current weather output. Use this if you only want the forecast data.
* `-f DAYS` Show the forecast for the next DAYS days.
* `-l` Show the city and region name of the forecast that was retrieved. Use this to verify the zip code that you are using is in fact your area.
* `-v` Print headers above all output.


Here is some example output from the script:

Default options:

    $ weather.py 24060
    55F | Drizzle

Forecast and location options:

    $ weather.py -lf2 24060
    Blacksburg, VA

    55F | Drizzle

    22 Apr 2008
      Low: 50F
      High: 60F
      Condition: Few Showers

    23 Apr 2008
      Low: 49F
      High: 71F
      Condition: AM Clouds/PM Sun
