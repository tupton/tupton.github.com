---
date: '2011-12-23 21:48:47'
layout: post
slug: local-backups-are-great
status: publish
title: Local Backups are Great
wordpress_id: '663'
categories:
- Howto
---


I use [SuperDuper!][sd] to make daily backups to my local hard drive. [Remote backup][tarsnap-writeup] is important, but it's always nice to have an easily-accessible way to recover from a hard drive disaster, especially when you need to fix the situation *quickly*.

[sd]: http://www.shirt-pocket.com/SuperDuper/SuperDuperDescription.html
[tarsnap-writeup]: http://www.thomasupton.com/blog/2010/12/automated-backups-with-tarsnap/

## Your Backup Device

Choosing the right hard drive to use for your local backup isn't a difficult choice, but it is an important one.  Sometimes you can find great deals on good brands (Western Digital or Seagate, to name a couple brands that I have used and liked) on [Newegg][ne]. The requirements for a hard drive really depend on how you want to use it.

[ne]: http://www.newegg.com/

If you want a hard drive dedicated to backup, any drive with a relatively quick speed (I would go with 7200 RPM or higher) and a size that's at least a bit bigger than the drive you're backing up will be a great option.

If you want to use a hard drive for other things – to store your media or back up another drive – you'll obviously need something bigger. I have a 1 TB drive that I have split into two volumes. One volume is a bit bigger than my MacBook Pro's hard drive (128 GB)  and I use it to hold my backups; the other takes up the rest of the space and is used to store all of my music, movies, and photos.

Solid state drives are another option, but I've found that the extra cost isn't worth the speed benefits. I would absolutely recommend a solid state drive to use in your main computer on a daily basis – I have a 128 GB solid state drive in my MacBook Pro and it's noticeably faster than the mechanical hard drives I've used in MacBooks past. However, when it comes to something like backups, where you're mostly writing to it in a situation that isn't time-sensitive (e.g. at night), I think mechanical drives are the best option because you can get much more storage space for much less money.

## Software That Helps You

I mentioned earlier that I use SuperDuper!, but there are many other backup applications out there. Some of them may fit your specific needs better. The key feature is to have an automated and *bootable* copy of your hard drive ready to go at a moment's notice.

I admittedly have not given Time Machine a fair shot, but that's because I don't have a use case for its main draw. "Going back in time" to an earlier version of a file is not something I need to do on a regular basis. And now with Lion's Versions feature, I don't need a whole backup just to keep recent versions of my files around.

Another option is [Carbon Copy Cloner][ccc]. It makes bootable backups and can be scheduled to back up on a daily basis. It's free to try; donations are requested if you continue to use the product.

[ccc]: http://www.bombich.com/ccc_features.html

I love SuperDuper! because it's so simple. The interface tells you in plain English what is going to happen. I set up my nightly backups one time a couple years ago and the only time I ever see the app again is when I am traveling without my hard drive. When a backup fails, the application stays open to tell you what went wrong. That rarely happens though: usually an incremental backup occurs at 3am that takes less than ten minutes to complete. When it's done, my data is safe and sound in two places.

The successful backups are bootable, meaning that you can just choose the backup volume as the boot volume when you [hold Option after you turn your computer on][startup-manager-kb] and restore from there. If you just want to restore certain files instead of everything, the drive is right there in Finder for you to navigate as usual.

[startup-manager-kb]: http://support.apple.com/kb/HT1310

## Backup Is Not a Daunting Task

Almost none of my "non-technical" friends (i.e. friends who do other things in their lives besides fiddle with computers) back up their computers. And yet I know that each one of those friends would be devastated if their hard drive crashed and they lost their music or photos. Local backups are the simplest way to avoid this, and it's so simple to set up:

* Buy a hard drive (7200 RPM and at least as big as the drive you want to back up) and [format it using OS X's built-in Disk Utility][format-guide].
* Set up a [backup application][sd] to do daily backups of your entire disk.

[format-guide]: http://www.iclarified.com/entry/index.php?enid=1075

That's it. Back up your data. It's so simple and so useful.
