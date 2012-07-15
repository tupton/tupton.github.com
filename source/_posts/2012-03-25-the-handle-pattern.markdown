---
date: '2012-03-25 13:12:06'
layout: post
slug: the-handle-pattern
status: publish
title: The Handle Pattern
wordpress_id: '732'
categories:
- Code
- Webapps
---

I'm not entirely sure if there's a better name for this pattern that already exists, but I like "handle pattern" to describe this method of keeping track of and managing "subscriptions".


    
    
    <code data-language="javascript">
    var subscriber = (function() {
    	
        var _listeners = {};
    	    
        var s = {
            // Listen to a channel and call a callback when that channel fires
            listen: function(channel, cb) {
                // Add the callback to the list of listeners on the given channel
                _listeners[channel] = _listeners[channel] || [];
                _listeners[channel].push(cb);
                
                return {
                    // Return an object that can be used to remove the callback from the channel
                    unlisten: function() {
                        // Remove the callback from the list of listeners on that channel
                        _listeners[channel].splice(_listeners[channel].indexOf(cb), 1);
                    }
                }
            },
            
            // Manually fire events on a given channel.
            publish: function(channel) {
                _listeners[channel] = _listeners[channel] || [];
                _listeners[channel].forEach(function(cb) {
                    cb();
                });
            }
        };
        
        return s;
    })();
    	
    var h = subscriber.listen('update', function() {
        console.log('The update event was fired!');
    });
    
    subscriber.publish('update'); // > "The update event was fired!"
    
    h.unlisten();
        
    subscriber.publish('update'); // > <no output>
    </code>
    


	
When you have an event-driven application, like a Javascript app that performs actions based on user interaction or based on back end "pushes" to a listening front end, you often have a central "publisher" that handles firing events when certain actions occur. It makes sense to have a static `listen` function that takes a "channel" and a callback function to call when that channel gets updated. The problem comes when you have to decide how to *stop* listening to that channel. If you go with a static `unlisten` function on the publisher with the same signature as `listen` (the channel and callback), you need to keep track of which callback is listening to which channel, and it can get messy.

Instead, `listen` can return a `handle`, which is just an object that contains a method `unlisten` that knows exactly how to stop listening on the specific channel and with the specific callback that was given to `listen`. Then, the caller just needs to keep track of the return values of `listen` (as opposed to the *arguments* to `listen`) in order to be able to `unlisten` later.

The [Dojo][dojo] [Stateful][dojo.stateful] interface uses this pattern to watch values on objects. If you watch a property, a callback can be fired each time that property changes. The return value of `watch` is a handle that can be used to stop watching that particular property value.

[dojo]: http://dojotoolkit.org/
[dojo.stateful]: http://dojotoolkit.org/reference-guide/1.7/dojo/Stateful.html

Feel free to [play around with this code on JSFiddle][fiddle].

[fiddle]: http://jsfiddle.net/tupton/nZKQz/
