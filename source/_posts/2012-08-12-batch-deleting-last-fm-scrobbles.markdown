---
layout: post
title: "Batch Deleting Last.fm Scrobbles"
date: 2012-08-12 20:06
comments: true
categories:
- Code
- Howto
- Music
---

I recently came home from a trip and synced my iPhone in iTunes in an attempt to scrobble the music
that I listened to on that trip. I use [Melo][m] to scrobble tracks played in iTunes, and it usually
works quite well because I never know it's there.

[m]: http://meloapp.com/faq/

I'm not entirely sure if Melo, iTunes, or [Last.fm][lfm] is to blame, but I ended up with a large amount of
scrobbles from a repeated handful of songs. I have over 75,000 tracks scrobbled in Last.fm, but I
use their recommendations and like to look at my stats, so artificially inflating my scrobble
count with three artists was extremely undesirable.

[lfm]: http://last.fm/

I had 59 pages of unwanted scrobbles; I needed to quickly delete nearly 3000 scrobbles. Last.fm
doesn't have a way to batch delete (or otherwise manage) your scrobble tracks, so I manually clicked
all the delete links on the first page.

That got old before I had even deleted ten scrobbles. I figured out a way to programmatically and
quickly delete a page of scrobbles. I still have to manually get to each page, but this makes it
much easier.

-----

*UPDATE 2017-05-09*:

Thanks to a reader, I was informed that the site design has changed yet again, requiring a new selector for the delete button. The new code should now be as follows (thanks AN and Slype!):

``` javascript
jQuery('.chartlist button.dropdown-menu-clickable-item[type="submit"]').each(function(_, c) {
    c.click();
});
```

Again, I'll leave the old code up for posterity.

-----

*UPDATE 2015-10-08*:

Since the new Last.fm redesign recently went live after being in beta for a while, it looks like the selector in the above code needs to change. Thanks to [@jayholler][], the code should now be:

``` javascript
jQuery('.chartlist button.chartlist-delete-button').each(function(_, b) {
    b.click();
});
```

  [@jayholler]: https://twitter.com/jayholler/status/652226890338439168

I am leaving the old code up for posterity and just in case the old site is still accessible somewehere.

-----

``` javascript
jQuery('#deletablert a.delete').each(function(_, a) {
    a.click();
});
```

I just open up the console with `⌘-⌥-I`, paste in that snippet of code, and hit enter. Here is what
it looks like in action.

![Batch deleting Last.fm scrobbles.](http://farm9.staticflickr.com/8282/7770297078_73fafc7541_o_d.png "Last.fm Batch Delete")

Since [jQuery][jq] is already embedded in Last.fm's pages, I just select all the delete links and
emit a click on each one. The entire page is deleted in a few seconds. When it's done, I can click
the link to the previous page and repeat.

[jq]: http://jquery.com/

I don't expect this will be very useful to anyone but myself for the next ten minutes, but it could
come in handy in case you've been listening to too much Carly Rae Jepsen.

