---
date: '2012-05-20 14:13:08'
layout: post
slug: syntastic
status: publish
title: Syntastic
wordpress_id: '750'
categories:
- Code
- Programming
---

I love writing with `vim`, but — for many valid reasons — some people are averse to it. I think the
steep (–ish) learning curve has something to do with it, but I think some people really don't want
to give up their IDE. The thing is, `vim` isn't *just* a text editor. You can extend it to do pretty
much anything, including features that normally show up in IDEs.

I recently started using [syntastic][], which is a `vim` plugin that runs linters or syntax checkers
and displays warnings and errors right in your window. IDEs like Eclipse or Xcode provide this
syntax-checking for a couple of languages, but syntastic supports many languages and linters out of
the box with pluggable support for nearly any language.

[syntastic]: https://github.com/scrooloose/syntastic

## Setup

If you use [pathogen][] like every sane `vim` user does, installing syntastic is just like any other
plugin. Clone the repository or create a submodule in `bundle/syntastic` and you're ready to go.

[pathogen]: https://github.com/tpope/vim-pathogen

I mainly work with Javascript, Python, HTML, and CSS, so that's what I'll talk about. It's worth
checking out the syntax checkers in `bundle/syntastic/syntax_checkers/`, even if you just look at
the list of files there. You might have to dive into the files to figure out which linters are
actually supported as it's not clearly documented.

Before we get to configuring individual syntax checkers, syntastic itself has a couple of
configuration options. Syntastic has what it refers to as a "mode map", which is basically just a
way to configure which file types are checked. Here is the relevant config option from [my
vimrc][vimrc]:

[vimrc]: https://github.com/tupton/vim-support/blob/master/vimrc#L302

``` vim
	" On by default, turn it off for html
	let g:syntastic_mode_map = { 'mode': 'active',
		\ 'active_filetypes': [],
		\ 'passive_filetypes': ['html'] }
```

The 'mode' option set to active means that syntastic is on by default, so we can leave the list of
'active_filetypes' empty. The 'passive_filetypes' names filetypes that syntastic does not attempt to
check. I don't check HTML files because I mainly work with templates that either can't or won't
validate with most HTML syntax checkers. If templates use non-standard attributes, it's hard for a
syntax checker to do its job.

### Python

Setting up syntastic for use with python was extremely easy. Just install [pyflakes][] with
`pip install pyflakes` and set up some related options. I ignore certain errors regarding
whitespace, indentation, and end-of-line backslashes, but those are all customizable. See the [pep8
error code documentation][pep8-errors] for an explanation of the error codes.

``` vim
    " Use flake8
    let g:syntastic_python_checkers = ['flake8']
    let g:syntastic_python_flake8_args = '--ignore="E501,E302,E261,E701,E241,E126,E127,E128,W801"'
```

[pyflakes]: http://pypi.python.org/pypi/pyflakes
[pep8-errors]: http://pep8.readthedocs.org/en/latest/intro.html#error-codes

### Javascript

There are a number of Javascript linters, including [JS Lint][jslint] and Google's [Closure
linter][clinter], but I decided to use the communtiy-driven [JS Hint][jshint].  JS Hint can be
configured with a jshintrc, which just is a JSON object that contains options. There are two
categories of options: those that enforce stricter rules than the defaults and those that relax
default checks. The [options page of the jshint website][jshint-options] does a great job of
explaining each option. You can [view my jshintrc on my Github page][jshintrc]. I added a few
"enforcing" options, but the only "relaxing" option I use is `sub`, which allows
subscript notation when accessing properties, e.g. `this['domNode']` as well as `this.domNode`.

You can tell syntastic to use jshint with the following config option:

``` vim
" Use jshint (uses ~/.jshintrc)
let g:syntastic_javascript_checkers = ['jshint']
```

[jslint]: http://www.jslint.com/
[clinter]: https://developers.google.com/closure/utilities/
[jshint]: http://www.jshint.com/
[jshint-options]: http://www.jshint.com/options/
[jshintrc]: https://github.com/tupton/dotfiles/blob/master/jshintrc

## Usage

![syntastic and underscore.js](http://farm8.staticflickr.com/7085/7235544916_6f48ae8f45_o_d.png)

By default, syntastic runs your file through the configured filetype's linter whenever the buffer is
written. If there are errors, they are highlighted inline. When your cursor is on a line with an
error, the description of the error is visible in the status line. You can use the `:Errors` command
to open a quickfix window with a list of all errors in the file. Pressing enter on an error will
take you to that line in the file. Syntastic uses [vim's signs][vim-signs], which means that a
gutter with a sign appears on the left side of every line with an error on it, making it easy to
scan the buffer for errors. You can change these symbols with some syntastic options.

``` vim
    " Better :sign interface symbols
    let g:syntastic_error_symbol = '✗'
    let g:syntastic_warning_symbol = '!'
```

[vim-signs]: http://vimdoc.sourceforge.net/htmldoc/sign.html

You can configure syntastic in many other ways, including telling it to check a buffer whenever it
is opened. I explored the help file (`:help syntastic`) when I first started using syntastic, and it
was incredibly helpful. I encourage anyone who uses syntastic to do the same.

Syntastic has already helped me avoid many typos and silly errors that would ordinarily be hard to
track down. It's an invaluable tool in my workflow.
