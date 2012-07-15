---
date: '2009-09-06 09:56:28'
layout: post
slug: updates-to-now-playing-perl-script
status: publish
title: Updates to Now Playing Perl Script
wordpress_id: '460'
categories:
- Code
- Music
- Software
tags:
- itunes
- perl
- script
- update
---

I recently updated my [Now Playing script][github] to work with OS X 10.6 Snow Leopard.

It seems as though [MacPerl][macperl] and [Mac::Processes][macprocesses] do not work in Snow Leopard's 64-bit environment. As a result, I went back to calling `osascript` directly in order to run the necessary AppleScript. You can look at the change history on [the project's GitHub page][github-change].

[github]: http://github.com/tupton/itunes-now-playing/tree/master
[github-change]: http://github.com/tupton/itunes-now-playing/commits/master/now-playing.pl

[macperl]: http://www.iis.ee.ethz.ch/~neeri/macintosh/perl.html
[macprocesses]: http://search.cpan.org/~cnandor/Mac-Carbon/README
