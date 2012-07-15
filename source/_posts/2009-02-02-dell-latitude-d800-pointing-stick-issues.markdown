---
date: '2009-02-02 00:48:46'
layout: post
slug: dell-latitude-d800-pointing-stick-issues
status: publish
title: Dell Latitude D800 Pointing Stick Issues
wordpress_id: '394'
categories:
- Hardware
- Howto
---

I recently installed [Ubuntu 8.10](http://www.ubuntu.com/) on my old Dell Latitude D800. I had not used that machine in a couple of years, and I had forgotten about a few hardware issues that it had. One of the most annoying issues was the "drifting" [pointing stick](http://en.wikipedia.org/wiki/Pointing_stick).

According to a number of posts I found when searching for solutions to the problem, the drifting pointing stick issue is a common one on many Dell models. It causes random and erratic mouse movement, making it nigh impossible to operate a graphical interface. I got around the issue when I was using this machine full time by disabling the pointing stick using the [Windows driver for the ALPS GlidePoint](http://support.dell.com/support/downloads/download.aspx?c=us&l=en&s=gen&releaseid=R89598&SystemID=LAT_PNT_PM_D600&os=WW1&osl=en&deviceid=2514&devlib=0&typecnt=1&vercnt=6&formatcnt=1&fileid=116869). Unfortunately, the system preferences offered by Ubuntu do not have an option to disable the pointing stick.

I found [a post on the Ubuntu forums](http://ubuntuforums.org/showthread.php?t=107521) from a user who had a similar problem to mine. A responder mentioned that he removed the keyboard and found that the pointing stick had its own leads that could be disconnected. I searched for the [Dell support document that matched my model](http://support.dell.com/support/edocs/systems/latd800/sm_en/keyboard.htm) and decided that I, too, could remove my keyboard and disconnect the pointing stick.

Unfortunately, I did not take any pictures of my machine when I had it opened. However, it is _very_ easy to remove the keyboard by following the instructions in the support document. Once the machine was open and the keyboard removed, I just ripped off the leads from the pointing stick where they attached back to the keyboard connector. To be safe, I taped the loose end with electrical tape. After I replaced the keyboard, the trackpad and keyboard both worked fine and were not marred by the erratic mouse movement that was caused by my pointing stick.
