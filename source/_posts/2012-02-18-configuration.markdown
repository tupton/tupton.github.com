---
date: '2012-02-18 20:43:38'
layout: post
slug: configuration
status: publish
title: Configuration
wordpress_id: '709'
categories:
- Code
- Howto
---

Starting a fresh environment on a new system isn't a very common or regular task, but when you do, it comes in handy to have a quick and easy way to do it. To help with this, I keep my [`vim` environment][vim-github] and my [environment dot files][dotfiles-github] in version control on Github.

[vim-github]: https://github.com/tupton/vim-support
[dotfiles-github]: https://github.com/tupton/dotfiles

I use [Pathogen][pathogen] to manage my `vim` plugins. I follow the examples in [this great article on plugin management with Pathogen][pathogen-git-manage] to keep most of my plugins as `git` submodules. Installing my `vim` environment is just a matter of cloning the `git` repository and symlinking my `vimrc`.

[pathogen]: https://github.com/tpope/vim-pathogen
[pathogen-git-manage]: http://vimcasts.org/episodes/synchronizing-plugins-with-git-submodules-and-pathogen/

I just spent some time cleaning up and organizing my dotfiles.  I wasn't even using my old dotfiles repository on GitHub, but I quickly rectified that. I now keep my dotfiles in `~/.dotfiles` and symlink them to `~`. There is a helper script in my [dotfiles repository][dotfiles-github] that sets up these symlinks.

Along with the usual `bashrc`, there are a few config files that aren't as common. My `ackrc` defines a few convenient options and ignores directories with built artifacts in them. My (old) `pythonrc` defines a tab-completion function, although this might be outdated at this point. There's a `tm_properties` config for [TextMate 2][tm2].

[tm2]: http://blog.macromates.com/2011/textmate-2-0-alpha/

Hopefully the fact that my configuration is out in the open will bring to light some useful features that I probably take for granted but you might not know about. So, go explore! Check out my `inputrc`, for example. I know that I discover new things almost every time I look at someone else's `vimrc` or `bashrc`.

Do you have any configuration tips or tricks? Let me know.
