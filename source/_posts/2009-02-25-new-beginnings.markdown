---
date: '2009-02-25 22:24:10'
layout: post
slug: new-beginnings
status: publish
title: New Beginnings
wordpress_id: '400'
categories:
- Design
- Howto
- Personal
---

I recently started working at [Mailtrust](http://www.mailtrust.com/ "Business Email Hosting & Webmail Services Mailtrust") in Blacksburg, VA. Mailtrust is a division of [Rackspace](http://www.rackspace.com/ "Dedicated Server - Dedicated Server Hosting - Managed Dedicated Server by Rackspace"), a company that has been experiencing some astounding growth recently. One of the companies that Rackspace recently acquired is [Slicehost](http://www.slicehost.com/ "Slicehost - VPS Hosting"). Last week, I decided to sign up for Slicehost hosting and move my website to this new server. I also decided to buy a new domain, **thomasupton.com**. I spent the last week or so setting up my server just how I wanted it.

I followed the excellent [Slicehost articles](http://articles.slicehost.com/ "Slicehost Article Repository - VPS setup, servers, Ruby on Rails, Django, PHP, DNS, Slicemanager and more") in order to get started. Setup is a breeze because you have total control over your server. When I screwed something up shortly after starting the setup process, I simply rebuilt and re-imaged my slice and started over.

The articles for [Ubuntu 8.10](http://www.ubuntu.com/getubuntu/download "Download Ubuntu Ubuntu") are presented in a useful order and detail how to set up and secure a new slice, install Apache to serve websites, and install MySQL and PHP to start getting things done.

By far the hardest part of setting all of this up was the migration of my [Wordpress](http://wordpress.org/ "WordPress › Blog Tool and Publishing Platform") installation. I spent a lot of time trying to export a MySQL dump of my existing Wordpress database and import onto my slice. It turns out that [Wordpress supports an extended XML-based format for importing and exporting](http://en.blog.wordpress.com/2006/06/12/xml-import-export/ "XML Import / Export « Blog « WordPress.com") almost every bit of user-created content, including posts, comments, and pages. Existing URL structures are preserved, and author data can be changed to an existing author in the new installation. This was *much* easier to do than my attempt to directly import a MySQL dump.

After I installed and migrated Wordpress, I decided that I wanted to redesign the look of the site. I wanted an article-centric design that took a minimalist approach to the extreme. What I needed was a skeleton Wordpress template that had useful, semantic markup that was easily customizable. Enter [Sandbox](http://www.plaintxt.org/themes/sandbox/ "Sandbox plaintxt.org"). After activating the theme, designing a layout purely in CSS was extremely easy, thanks in large part to the clear and concise HTML markup. I'm still tweaking the design here and there, and I want to add a dedicated Archives page, but I'm really happy with how the site has turned out so far.

As I mentioned a couple of weeks ago, my interest was rekindled in [Tumblr](http://www.tumblr.com/ "Tumblr") after the release of version five. Tumblr supports custom domains, so I've moved my tumblelog to [This Is Thomas](http://thisis.thomasupton.com/ "This Is Thomas").

I'm really hoping to have more time to blog about issues and topics that I care about now that I don't have class and homework to worry about. The site is continually undergoing changes in design and structure as I learn more about how to write and design more effectively.

Enjoy.
