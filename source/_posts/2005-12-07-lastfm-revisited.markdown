---
date: '2005-12-07 13:23:00'
layout: post
slug: lastfm-revisited
status: publish
title: Last.fm Revisited
wordpress_id: '49'
categories:
- Howto
- Music
- Software
- Websites
---

[![Recently Played Tracks - Generated HTML](http://static.flickr.com/20/71235012_5b230c6940_o.png)](http://www.flickr.com/photos/third/71235012/)

Earlier today, I posted about a [Last.fm](http://last.fm/) [recently played tracks image generator](http://www.audioscrobbler.net/wiki/index.php/RecentlyPlayedImageV2#arguments), but I expressed an interest in an HTML-generating method of showing these tracks.  Well, I [found one](http://www.audioscrobbler.net/wiki/RecentTracksOnYourBlog).

I'll outline the steps that I took to make this work, because that wiki is not so helpful.



	
  1. Go to [Feed Digest](http://feeddigest.com/).  This is a website that takes an RSS feed and parses it how you want into HTML.  The service also offers a mix of multiple feeds, but we don't need that for Last.fm.

	
  2. Create a digest from your Last.fm feed.  The format is http://ws.audioscrobbler.com/1.0/user/<username>/recenttracks.rss, where <username> is replaced by your Last.fm user name.

	
  3. Edit the template of your feed.  I used

    
    <li class="music">
    <span><a href="%URL%" >%TITLE%</a></span>
    </li>


for the per-item template and

    
    <p><a href="http://last.fm/user/Tupton">Recently Played Tracks</a></p>


for the header template.  This makes a list out of your tracks, allowing for better CSS styling.

	
  4. Get the code for your feed.  After creating the feed, you can get a Javascript implementation of your feed to include in your site.  All the code you need is offered on the site.

	
  5. Put the HTML in your web page.  I enclosed the Javascript include in a div, for greater editing ease.

	
  6. Style, baby!  Here is the CSS I used to make this work:



    
    #lastfm {
    padding: 3px;
    margin-top: 5px;
    border: #ddd solid 1px;
    background-color: #f8f8f8;
    font-size: 1em;
    }
     #lastfm a:hover {
    color: #fff;
    background-color: #39c;
    }
     #lastfm p {
    font-size: 12px;
    font-weight: bold;
    }
     #lastfm ul {
    margin-left: -40px;
    }
     #lastfm li.music {
    font-size: 10px;
    line-height: 2em;
    padding: 2px;
    margin-bottom: 3px;
    background-color: #fff;
    border-top: 1px #ddd solid;
    border-bottom: 1px #ddd solid;
    list-style-image:url('http://www.fiveuptons.com/thomas/images/audio.gif');
    }
     #lastfm li.music:hover {
    background-color: #39c;
    }
     #lastfm li.music:hover a {
    color: #fff;
    }
     #lastfm li.music a {
    display: block;
    }






The image I used for the list bullet was an edited image from the BBC.  Feel free to use it, but just save it on your own server so you aren't hot-linking to my images, please.  Also, a [Google search for speaker.gif](http://images.google.com/images?q=speaker.gif&hl=en) yields some nice results.  You can see the image I used is the third one there.

You can see the final result of what I did on my [main page](http://fiveuptons.com/thomas/).  Enjoy!
