---
date: '2006-02-06 23:35:00'
layout: post
slug: 5-easy-steps-to-keeping-some-of-your-music-on-your-laptop
status: publish
title: 5 Easy Steps To Keeping Some Of Your Music On Your Laptop
wordpress_id: '66'
categories:
- Howto
---

Last summer, I ran out of room on my Powerbook.  I woke up to a system alert telling me that there was no more room on my boot disk, and that I needed to make room.  I deleted some files to reclaim a few megabytes, but I needed a more permanent solution.  What was taking up all of my 60GB hard drive?  [Music](http://www.last.fm/user/TUpton/).  I ended up buying an internal hardrive and enclosure from [Newegg](http://www.newegg.com/), and it has been amazing.  I moved all my music and movies over to the external drive and have loads (~20GB) of space left on my Powerbook.

There is, however, a drawback.  If I don't have my hard drive plugged in, then I do not have access to my music.  After some careful consideration, and some prompting from this [AskMeFi thread](http://ask.metafilter.com/mefi/32031), I came up with what I believe to be a solution to keeping some music on my Powerbook while simultaneously keeping a majority of my library on my external drive.  The rest of this post is based on my answer in the AskMeFi thread.

When I unplug my external drive and open iTunes, the Music Library folder is automatically changed to ~/Music/iTunes.  Since the iTunes Library XML file is stored locally, I think iTunes automatically chooses the location of this file as its new music folder when the default is unavailable.  If I connect my external drive and restart iTunes, the folder changes back automatically.  This is the key to this whole method.

Make sure the option to copy music to your iTunes folder and the option to keep your iTunes folder organized are both checked.  This makes it far easier to keep track of what is stored locally as opposed to what is on the external drive.

Move all the music you want on the external drive to a directory on that drive and make that directory your iTunes folder. As I mentioned earlier, when I moved to an external drive, my iTunes library files stayed on my local drive so that when I opened iTunes I still had a listing of all my songs.

Keep / move the music you want on your Powerbook to your internal drive, into ~/Music/iTunes.  You may have to manually tell iTunes the location of the moved music if you already had it on the external, but that's just a sacrifice that has to be made, I'm afraid. You do this by double clicking a song in iTunes that you just moved, and then you can browse for the new file. I know of no batch method to do this procedure.  Maybe someone can come up with an Applescript that would automatically move the files to a new location _and_ tell iTunes that the file had moved?

Now, when you have your external hard drive plugged in, you'll be able to play music as usual. When you are not plugged in to your hard drive, all your songs will be listed in the library, but only the songs that you elected to keep on your internal drive will be playable.

If you want to put music on the external drive, make sure you're plugged in when you rip or add music. If you want music on your Powerbook, make sure you are not plugged in when you rip your music or add it to the library.



## Drawbacks


You have to tell iTunes where that locally placed music is located. That could get tedious if you want to have more than a few albums on your Powerbook's drive.  Fortunately, iTunes is scriptable, and I'm sure that it cannot be that hard to put something together that automates this task.  Another drawback that goes hand in hand with this one is moving music _back_ to the external drive when you run out of room on your local hard drive or decide that you don't want it on there anymore.

You have to pay attention to what drive you are adding music when adding. This shouldn't be too hard though.



## Advantages


This method is transparent. You shouldn't have to do anything more than decide what drive you want to put music on.

This is easy. There is no scripting, or advanced wishy-washy computer voodoo, and, besides having to tell iTunes where your moved music is, relatively work-free.

I have, in the course of writing this, realized that I have done this by accident in the past: I added some downloaded tracks without being plugged in to my external hard drive, and I went for weeks before realizing that the music was playing from my internal hard drive (I realized when I started playing music and my hard drive was not turned on.)  This method works, and it works with minimal effort.

Again, if anyone has any information or helpful comments on the state of a script to move the location of iTunes files, the sharing of that information would be greatly appreciated.

And yes, I know that this is not exactly five step by step instructions, but hey, it got your attention didn't it?
