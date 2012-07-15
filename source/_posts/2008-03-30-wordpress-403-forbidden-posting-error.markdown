---
date: '2008-03-30 00:35:13'
layout: post
slug: wordpress-403-forbidden-posting-error
status: publish
title: Wordpress 403 Forbidden Posting Error
wordpress_id: '191'
categories:
- Howto
- Software
tags:
- wordpress
---

In trying to post about [my modified Twitter stats script] [twitter stats], I ran into a [403 Forbidden](http://en.wikipedia.org/wiki/HTTP_403) error every time I tried to publish. I could write and publish test posts with minimal text, but I could not post what I had written about my scripts.

[twitter stats]: http://thomas.fiveuptons.com/blog/?p=188

At first I thought it was an outdated version of Wordpress that was responsible for the error. However, upgrading to 2.3 did not fix anything. I simply ignored the error and conceded that I would not be able to post anything for now.

[Wordpress 2.5] [wp] was released today, and I decided to try posting once more. I still got the same error. This time, I decided to [Google the issue] [google].

[wp]: http://wordpress.org/development/2008/03/wordpress-25-brecker/
[google]: http://www.google.com/search?q=You%20don%27t%20have%20permission%20to%20access%20%2Fblog%2Fwp-admin%2Fpost.php%20on%20this%20server.

The [results] [a small orange] [are] [wp support 1] [not] [wp support 2] [suprising] [wp support 3].

[a small orange]: http://forums.asmallorange.com/index.php?showtopic=3631&st=0&p=26154&
[wp support 1]: http://wordpress.org/support/topic/135394
[wp support 2]: http://wordpress.org/support/topic/117993?replies=2
[wp support 3]: http://wordpress.org/support/topic/37489

It seems that Wordpress has some parsing issues when posts contain certain PHP commands. Apparently, modifying the mod_security rules in .htaccess helps to resolve this issue.

Adding the following code to the .htaccess file in `wp-admin/` does the trick.

    
    SecFilterEngine Off
    
