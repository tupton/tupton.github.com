---
date: '2008-03-30 00:12:09'
layout: post
slug: twitter-stats
status: publish
title: Twitter Stats
wordpress_id: '188'
categories:
- Articles
- Code
- Interesting
- Websites
tags:
- Code
- twitter
---

**Update:** [TweetStats](http://www.tweetstats.com/) webifies a version of these Twitter statistics.



![Twitter stats, by hour.](http://www.thomasupton.com/wp/wp-content/uploads/2008/03/twitter_stats-300x154.png)

Damon Cortesi wrote a [pretty nifty script to grab some Twitter statistics] [Cortesi]. A bunch of people modified his script in order to [webify it] [Kellett], [plot the results using gnuplot] [McCracken], and [plot the results with the Google Charts API] [Barrett].

[cortesi]: http://dcortesi.com/2007/12/27/twitter-stats/
[kellett]: http://bradkellett.com/twitter_stats.html
[mccracken]: http://michael-mccracken.net/wp/2008/01/02/twitter-stats-in-svg-using-gnuplot/
[barrett]: http://iamthewalr.us/blog/2008/01/02/twitter-stats/

This last modification really intrigued me, as I know there are [a few wrappers for the Google Charts API] [google charts] available, including one for Python. I decided to modify [Colin Barrett's Python script] [Barrett] that uses the Google Charts API to use the [pygooglechart] [] module.

[google charts]: http://google-code-updates.blogspot.com/2008/01/chartmaker-tool-for-google-chart-api.html
[pygooglechart]: http://pygooglechart.slowchop.com/

So now instead of:

    if f[0] == "\n":
        s = "http://chart.apis.google.com/chart?cht=bvg&chbh;=15&chs;=600x300&chd;=t:%s&chxt;=y,x&
        chxr=0,0,%d&chtt;=%s"
        s = s % ( str(map(lambda x: x*100/max(v), v)).replace(" ", "")[1:-1], max(v), t )
        
we have:

    # If we are at a newline, we have just finished gathering data for a single chart
    if fields[0] == "\n":
        # Most of the time we are generating a vertical bar chart
        chart = GroupedVerticalBarChart(600, 300)
        # Set the y-axis range according to the current data
        chart.set_axis_range(Axis.LEFT, 0, max(chartData))
        
I also commented my modifications because I'm still on winter break and I felt like it.

You can [download my modified script here] [script]. Make sure you have the pygooglechart module and read the included README.txt file.

[script]: http://www.thomasupton.com/wp/wp-content/uploads/2008/01/pygooglechart_twitter_stats.zip
