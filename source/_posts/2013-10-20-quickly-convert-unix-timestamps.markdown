---
layout: post
title: "Quickly convert Unix timestamps"
date: 2013-10-20 10:03
comments: true
categories:
 - Code
 - Script
---

Inevitably, when dealing with time-related data, one will come across Unix timestamps. They're
great; there's no guessing the timezone or trying to parse difficult formats and they're generally
extremely useful.

Except they're not very readable to humans. I use [Epoch Converter][ec] *a lot* when I'm dealing
with time-series data, which seems to be fairly often recently. Anything involving a calendar or
picking a time range also usually involves timestamps. I thought there must be an easier way to
convert these into something that I can read without going to an external site and that doesn't
break down when I'm trying to do work with finnicky or non-existant network connection.

[ec]: http://www.epochconverter.com/

There is:

``` bash
    date -j -f "%s" "1381528800" +"%a %b %d %T %Z %Y"
```

An explanation: `-j` tells `date` not to try to set the system date. `-f` describes the format of
the input - a timestamp, of course - and the input itself follows. The string after `+` is the
output format. The output of the above is as follows.

    Fri Oct 11 17:00:00 CDT 2013

You can use `xargs` to pipe input in. Note that the `-J` option might be different on systems that
are not OS X.

``` bash
    echo -n "1381528800" | xargs -J {} date -j -f "%s" {} +"%a %b %d %T %Z %Y"
```

Like I seem to do with a lot of my utility scripts, I added a workflow to Alfred. You can [get it
here][workflow]. Simply invoke Alfred, type "ts" followed by a space and the timestamp you want to
convert. The readable date will be posted as a notification (Growl or Notification Center,
configurable in the Advanced section of the Alfred settings) and copied to the clipboard.

[workflow]: https://dl.dropboxusercontent.com/u/5753691/Convert%20timestamp%20to%20date.alfredworkflow
