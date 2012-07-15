---
date: '2010-12-11 14:42:56'
layout: post
slug: pastebin-from-the-commandline
status: publish
title: Pastebin From The Command-line
wordpress_id: '598'
categories:
- Articles
- Code
- Howto
- Software
- Webapps
---

[Pastebin.com][pb] is a utility for sharing snippets of text with anyone. The service is simple: go to [pastebin.com][pb], paste your text, click submit, and share the link. It's useful for sharing time-saving snippets with you team, or for an impromptu, informal code review of changes that for one reason or another haven't been committed yet (e.g. a design decision for which the author wants early feedback.)

[pb]: http://pastebin.com/

> *10:56:55 AM* **Mike Bulman**: im gonna try to find one, but if it doesn't exist im gonna write:  a service/app for osx that will post whatevers on the clipboard to pastebin and give you a url

A couple of days ago, my friend [Mike][mb] suggested that he wanted a utility that would make it easy to post to pastebin without having to break your workflow and go to the site. I thought "that sounds fun," and went to work that evening.

[mb]: http://twitter.com/#!/mikebulman

Pastebin has a [simple RPC API][pb-api] for posting content, and it's perfect for `curl`. The very first version of the pastebin script was this simple `curl` script.

[pb-api]: http://pastebin.com/api.php


    
    
    <code data-language="shell">
    #! /usr/bin/env bash
    curl -s -X POST -d "paste_code=$1" "http://pastebin.com/api_public.php" | pbcopy
    </code>
    


    
`paste_code` is the POST parameter that the pastebin API expects to contain pasted content, so we just POST that to the public API page. The output of a successful pastebin API call is the URL for the paste, so we pipe that to OS X's clipboard for easy pasting. Right from the beginning, this is clearly an OS X-specific script, but I don't think it would be that hard to interact with the Linux system clipboard via `xclip`.

Making this work for text from `stdin` instead of as an argument was easy, too.


    
    
    <code data-language="shell">
    #! /usr/bin/env bash
    to_paste=`cat /dev/stdin`
    curl -s -X POST -d "paste_code=$to_paste" "http://pastebin.com/api_public.php" | pbcopy
    </code>
    



Both Mike and I recently started using [Jumpcut][jc], a minimal clipboard manager for OS X, so I had the idea of copying the actual text that was posted to pastebin before copying the URL so both would be readily available via Jumpcut. I tried adding `echo "$to_paste" | pbcopy` before the `curl` call, but it didn't always work. I didn't actually find any documentation on it, but it seems that `pbcopy` does some sort of buffering so that too much text isn't copied in quick succession. Adding a very hacky `sleep` call fixed it, so the resulting script became the following.

[jc]: http://jumpcut.sourceforge.net/


    
    
    <code data-language="shell">
    #! /usr/bin/env bash
    to_paste=`cat /dev/stdin`
    echo "$to_paste" | pbcopy
    sleep 1
    curl -s -X POST -d "paste_code=$1" "http://pastebin.com/api_public.php" | pbcopy
    </code>
    


    
Having a commandline script is all well and good, but it's on about the same usability level as using pastebin.com: instead of switching to a browser, you switch to a terminal, but you still have to paste content and get away from you original context. This is where OS X Services come into play.

The OS X Service menu is often neglected, but it's very useful. There are some neat built-in services, like looking a word up in Dictionary or composing a new email with the selection. With tools like  [Automator][auto] or [ThisService][ts], creating your own service is easy. I created an Automator workflow that assumed the script was symbolically linked to `/usr/local/bin/pastebin` and called the script. You can easily add a keyboard shortcut to a Service in the System Preferences Keyboard Shortcuts preference pane.

[auto]: http://www.macosxautomation.com/automator/
[ts]: http://wafflesoftware.net/thisservice/

I cleaned up the script by adding a few command line options using the `getopts` `bash` built-in. This was pretty useful and easy to use. I'm not a very proficient `bash` scripter, but I was easily able to add what I needed, including making the ability to copy the text optional and allowing a file to be used in place of `stdin`. [Gina Trapani][gt]'s excellent [todo.txt][td] script was helpful in figuring out the correct syntax.

[gt]: http://ginatrapani.org/
[td]: https://github.com/ginatrapani/todo.txt-cli


    
    
    <code data-language="shell">
    while getopts ":cf:h" Option
    do
        case $Option in
            c )
                PASTEBIN_COPY=1
                ;;
            f )
                PASTEBIN_FILE=$OPTARG
                ;;
            h )
                usage
                ;;
        esac
    done
    </code>
    


    
I wrapped the script and the Automator workflow and [added it to github][gh]. You can [view the original bash script][bash-script] in its latest form if you're interested.

[gh]: https://github.com/tupton/pastebin-cl/
[bash-script]: https://github.com/tupton/pastebin-cl/blob/8ff3cdda9e0027c41416b285f2822781a0ba4b5e/pastebin.sh

All this was well and good, but a couple of things bothered me. I didn't like that `sleep` call, and I knew that the pastebin API offered a few options that I wasn't allowing users of this script to take advantage of. So I did what any normal developer would do: I rewrote the script in python. The [original version of the script][python-script] was just a straight "port" of the `bash` script, but I had plans for more.

[python-script]: https://github.com/tupton/pastebin-cl/blob/d462c57542e98f23c83dc6388e1e5ebbafe04c94/pastebin.py

I have experience with [simple python scripts][weather], but I decided I should try my luck using classes in order to wrap the pastebin API. It was really easy. The beginning of the class looked like the following.

[weather]: https://github.com/tupton/python-yahoo-weather


    
    
    <code data-language="python">
    class Pastebin(object):
        """A wrapper for the pastebin API"""
    
        pastebin_api = "http://pastebin.com/api_public.php"
    
        def __init__(self, paste_code, paste_name=None, paste_email=None, paste_subdomain=None,
                paste_private=False, paste_expire_date=PASTEBIN_EXPIRE_NEVER, paste_format=None):
    
            self.set_paste_code(paste_code)
            self.set_paste_name(paste_name)
            self.set_paste_email(paste_email)
            self.set_paste_subdomain(paste_subdomain)
            self.set_paste_private(paste_private)
            self.set_paste_expire_date(paste_expire_date)
            self.set_paste_format(paste_format)
    </code>
    



Then all I had to do was use the class.


    
    
    <code data-language="python">
    pastebin = Pastebin(paste_code=lines, paste_name=opts.paste_name, paste_email=opts.paste_email,
                paste_subdomain=opts.paste_subdomain, paste_private=opts.paste_private,
                paste_expire_date=opts.paste_expire_date, paste_format=opts.paste_format)
    response = pastebin.paste()
    return response
    </code>
    


        
I have no idea if getters and setters are very "pythonic," but they make sense to me, even if I'm only using them internally for this script. I used a pretty interesting trick to build the POST data string. I put the necessary data into a dictionary, but I needed to build a proper parameter string.


    
    
    <code data-language="python">
    return "&".join([k + "=" + str(v) for (k, v) in params.iteritems()])
    </code>
    


    
I love the syntax used for "inline" for-loops here. The extra `str()` call was used just in case a parameter (like expiration date) was accidentally interpreted as a number.

I updated the [README][r] to reflect the new options. Thankfully, only the symbolic link to `/usr/local/bin/pastebin` had to be updated in order to keep the workflow working. Using the pastebin script couldn't be easier:

 1. [Download the script source][dl], extract it, and put it somewhere that makes sense, like `~/scripts`.
 2. `ln -s ~/scripts/pastebin-cl/pastebin.py /usr/local/bin/pastebin`
 3. Open the workflow in `automator/Paste to pastebin.com.workflow` and save it as a service, which should just be as easy as ⌘-S.
 4. Open System Preferences > Keyboard, then Keyboard Shortcuts, then Services. Make sure that the "Paste to pastebin.com" service is checked. Optionally, you can give it a keyboard shortcut.
 5. Highlight some text in any OS X application and choose "Paste to pastebin.com" from the application menu's "Services" submenu or use your keyboard shortcut. Now the pastebin URL is on you clipboard, ready to be pasted into a chat room or in a browser.

[dl]: https://github.com/tupton/pastebin-cl/archives/master
[r]: https://github.com/tupton/pastebin-cl/blob/master/README.md

Hopefully this script will be useful to developers all over. If you have any ideas about making this more portable (i.e. for use on Linux or Windows), let me know in the [comments][c].

[c]: #respond
