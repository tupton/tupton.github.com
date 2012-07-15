---
date: '2006-02-02 02:07:00'
layout: post
slug: gmail-saved-searches-improvements
status: publish
title: Gmail Saved Searches Improvements
wordpress_id: '65'
categories:
- Code
- Howto
- Software
---

Persistent.info's [Gmail Saved Searches](http://persistent.info/archives/2005/03/01/gmail-searches) ([right click to install](http://persistent.info/greasemonkey/gmail.user.js)) is an amazing [Greasemonkey](http://greasemonkey.mozdev.org/) script.  It allows you to set up searches that live constantly (via a cookie) in Gmail's sidebar, and, with the latest version, lets you save your recent searches.  This script also comes with three default searches, including a search for last week's email.  The thing is, it's not really a search for Last Week; it just looks at email received in the last seven days.

I decided to fix that.  The following function is close to the end of the script, and my code changes are in emboldened red.


    
    PersistentSearch.prototype.getRunnableQuery = function() {
      var query = this.query;
      if (this.isSearchModifier()) {
        query = query.substring(2);
      }
      var today = new Date();
      var yesterday = new Date(today.getTime() - ONE_DAY);
      var oneWeekAgo = new Date(today.getTime() - (today.getDay() <span class="warning">+ 7</span>) * ONE_DAY);
      <span class="warning">var lastSunday = new Date(today.getTime() - (today.getDay() - 1) * ONE_DAY)</span>;
      query = query.replace(/:today/g, ":" + getDateString(today));
      query = query.replace(/:yesterday/g, ":" + getDateString(yesterday));
      query = query.replace(/:oneweekago/g, ":" + getDateString(oneWeekAgo) <span class="warning">+ " before:" +
      getDateString(lastSunday))</span>;
      return query;
    }


Since Sunday is "0" in the JS Date object, all I had to do was take (today+7) away from the (current time * ONE_DAY) in order to get the earliest Sunday, and take (today-1) from the (current time * ONE_DAY) in order to get this past Sunday.  The extra "1" is there because the `before:` search operator is exclusive, unlike the `after:` operator.  All that was left was to modify the inserted search string.  Now, the Last Week search returns all email between the past two Sundays.

The new script is available [here](http://fiveuptons.com/thomas/gm) ([right click to install](http://fiveuptons.com/thomas/gm/gmailsavedsearches.user.js).)
