---
layout: post
title: "Remove leading whitespace"
date: 2013-10-13 14:28
comments: true
categories:
 - Code
 - Productivity
---

Sometimes it's useful to copy some code from a file and paste it somewhere else as an example. For
instance, I like to write code in a [jsfiddle][jsfiddle] and then paste a relevant subset of that
code to a [Stack Overflow][so] answer. I have also copied code from a project file to paste into
HipChat to quickly explain something to a coworker, or to paste into a JIRA ticket as a comment.

[jsfiddle]: http://jsfiddle.net/
[so]: http://stackoverflow.com/

If you copy code from the middle of a file, there's usually some leading whitespace on all lines
that you do not want to preserve in the context into which you are pasting the code. The problem is
that you don't want to get rid of *all* leading whitespace on *all* lines; you want to keep the
indentation intact. I usually get rid of the unwanted whitespace by manually deleting it
if it's only one or two lines or by pasting into a new `vim` buffer and using `<<` to shift the text
over as much as desired.

To illustrate, I have this text:

``` javascript
        var flatten = function(result, next_array) {
            console.log('current result', result);
            return result.concat(next_array);
        };

        [1, [2], [3, 4]]
            .reduce(flatten, []);
```

and I want this text:

``` javascript
    var flatten = function(result, next_array) {
        console.log('current result', result);
        return result.concat(next_array);
    };

    [1, [2], [3, 4]]
        .reduce(flatten, []);
```

## Remove leading whitespace

I knew there must be a way to remove the shortest leading whitespace from all lines
programmatically, but I'm not familiar enough with `awk`, `sed`, or shell scripting in general to
tackle the problem. I [asked the question on Stack Overflow][soq] and got a few great answers. I
ended up accepting [the single process `awk` version][soa].

[soq]: http://stackoverflow.com/questions/19328975/remove-shortest-leading-whitespace-from-all-lines/19332908
[soa]: http://stackoverflow.com/questions/19328975/remove-shortest-leading-whitespace-from-all-lines/19332908#19332908

If you use OS X, the built-in `awk` will not work with the given solution. If you use
[Hombrew][brew], fixing that is just a simple matter of `brew install gawk` and using `gawk` instead
of `awk`.

[brew]: http://brew.sh/

The given solution has a great explanation and works fine, but I made one addition. If the input is
a single line with *no* leading whitespace, the script fails. I fixed this with `if (!s) s=0;` at
the beginning of the `END` block.

The final version of my command looks like this. I've added some comments to explain what's going
on.

``` bash
    gawk  -F '\\S.*' \              # The awk field separator is everything after the first non-whitespace character, inclusive
    '{                              # The first block of the awk program
        l=length($1);               # The length of the first field, the leading whitespace
        if(l>0)                     # If the length of whitespace is non-zero,
            if(NR==1)               # and this is the first record,
                s=l;                # make 's', the number of whitespace characters, equal to its length
        else s=s>l?l:s;             # otherwise, make s the shorter of itself and the current whitespace
        a[NR]=$0                    # Index the entire line in an array by line number
    }                               # End of the first block of the awk program

    END{                            # Start the END, printing block of the awk program
        if(!s)s=0;                  # Make sure we always have a value for s
        for(i=1;i<=NR;i++){         # Loop over all records
            sub("^ {"s"}","",a[i]); # Substitute 's' whitespace characters with nothing
            print a[i];             # Print the line after substition
        }                           # End of the loop over records
    }'                              # End of the END block of the awk program
```

It is worth noting that this probably only work on text with spaces for whitespace, not
tabs or mixed whitespace.

## Integrate with an Alfred workflow

The ability to remove the shortest leading whitespace with a shell command is great, but I really
wanted a way to do this quickly with text on the clipboard. [Alfred workflows][aw] make that
possible.

[aw]: http://support.alfredapp.com/workflows

I created a simple three step workflow with a hotkey trigger, a script action, and a clipboard
output. You can [download it here][paste-wf] and use it with Alfred 2.

[paste-wf]: https://dl.dropboxusercontent.com/u/5753691/Paste-shifted-text.alfredworkflow

And now you can copy code in context and paste it with no leading whitespace wherever you want!
