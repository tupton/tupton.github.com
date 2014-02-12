---
layout: post
title: "Migrating from Pathogen to Vundle"
date: 2014-02-11 19:41
comments: true
categories:
 - Howto
 - Code
---

For [a while now][blog-config], I've been using [Pathogen][pathogen] to manage my `vim` plugins as bundles. I thought I was making things easier for myself by [using `git` submodules to help organize those plugins][pathogen-submodules], but submodules aren't the best method for deploying a `vim` environment in multiple places.

[blog-config]: http://blog.thomasupton.com/2012/02/configuration/
[pathogen]: https://github.com/tpope/vim-pathogen
[pathogen-submodules]: http://vimcasts.org/episodes/synchronizing-plugins-with-git-submodules-and-pathogen/

Fortunately for me, other, smarter people did this same thing and decided to fix it. This [blog post by James Lai][lai] details moving from almost the exact same system as mine to [Vundle][vundle], a newer and very `vim`-centric way of managing plugins. You list your plugins in your `vimrc`, you update them with `vim` (even though they are managed with `git`), and your `vim` environment is consistent everywhere.

[lai]: http://jameslaicreative.com/moving-up-upgrading-from-pathogen-to-vundle/
[vundle]: https://github.com/gmarik/vundle

I just migrated from Pathogen to Vundle, and I want to document the process.

First things first: clone Vundle into your `bundle/` directory.

``` bash
❯ git clone https://github.com/gmarik/vundle.git bundle/vundle
```

The [Vundle quick start guide][vundle-qs] does a great job of getting you started, so if you're just looking to use Vundle without any prior bundle management, I would start there. If you were managing your `vim` plugins with Pathogen and `git` submodules, the switch to Vundle is straightforward but requires a few more steps.

[vundle-qs]: https://github.com/gmarik/Vundle.vim#quick-start

At the top of [my `vimrc`][my-vimrc] I added a new Vundle-specific section and added the code from the quick start guide.

``` vim
" Of course
set nocompatible

" Required Vundle setup
filetype off
set runtimepath+=~/.vim/bundle/vundle
call vundle#rc()

Bundle 'gmarik/vundle'

```

[my-vimrc]: https://github.com/tupton/vim-support/blob/master/vimrc

Next, I wanted to add all the bundles I already use. `git submodule foreach` can actually help here.

``` bash
❯ git submodule foreach git remote -v
Entering 'bundle/airline'
origin  https://github.com/bling/vim-airline.git (fetch)
origin  https://github.com/bling/vim-airline.git (push)
Entering 'bundle/characterize'
origin  https://github.com/tpope/vim-characterize.git (fetch)
origin  https://github.com/tpope/vim-characterize.git (push)

[...]
```

I then just copied the GitHub user and repository name (the path portion of the remote URL minus ".git") and passed that to Vundle's `Bundle` command.

``` vim
" Better status line
Bundle 'bling/vim-airline'

" ga for character descriptions
Bundle 'tpope/vim-characterize'

[...]
```

Now we need to remove the existing submodules. One of the big pains of using `git` submodules is removing them when you no longer need them. This made trying out plugins harder than it needed to be, and it makes the transition to Vundle a bit more complicated.

I reference [this Stack Overflow post about removing `git` submodules][so-remove-git-submodule] every time I need to remove a plugin I was just trying out without fail. The command to remember is `git submodule deinit`. You can `deinit` all submodules in a given directory all at once.

[so-remove-git-submodule]: http://stackoverflow.com/questions/1260748/how-do-i-remove-a-git-submodule

``` bash
❯ git submodule deinit bundle/
Cleared directory 'bundle/airline'
Submodule 'bundle/airline' (https://github.com/bling/vim-airline.git) unregistered for path 'bundle/airline'
Cleared directory 'bundle/characterize'
Submodule 'bundle/characterize' (https://github.com/tpope/vim-characterize.git) unregistered for path 'bundle/characterize'

[...]
```

Then you need to explicitly remove all the bundles. Since Vundle is already in `bundle/vundle`, we need to remove each separate plugin bundle directory instead of blowing away the entire `bundle/` directory.

``` bash
❯ git rm bundle/airline bundle/characterize [...]
rm 'bundle/airline'
rm 'bundle/characterize'

[...]
```

It woud be a good idea to commit your staged changes here, which at this point should just be submodule removal.

If you were using Pathogen, don't forget to remove any setup from your `vimrc`.

``` vim
" Store pathogen itself in bundle/
runtime! bundle/pathogen/autoload/pathogen.vim

" Start it up
silent! call pathogen#infect()
silent! call pathogen#helptags()
```

It also helps to add `bundle/` to your `.gitignore`. Vundle now puts all plugins there, but you don't have to manually manage them any more.. Just add `bundle/**` to your `vim` environment's `.gitignore` file.

Now, open up `vim` and run `:BundleInstall`. All your `vim` bundles will be cloned and managed by Vundle, and you don't have to worry about updating submodules and running `git submodule foreach` everywhere you have a `vim` environment.

To update and migrate any existing `vim` environments on other machines, `git pull` in the migration changes, which shouldn't conflict as long as you were up to date to the commit before the migration. Then clone Vundle with `git clone https://github.com/gmarik/Vundle.vim.git bundle/vundle` and install your plugins with `vim +BundleInstall +qall`.

If you *were not* up to date to right before your migration changes, you'll probably have to manually remove all submodules by following the instructions above and resolve some conflicts when pulling changes in. Since you're trying to delete all of `bundle/`, this should be relatively painless: just delete `bundle/` and `git rebase --skip` any submodule update commits.

And now you're using Vundle! Add some new bundles into your `vimrc`, run `:BundleInstall`, and you're up and running. I found some that I'm going to try out in [joegoggins's vimrc][joegoggins] and in [the Vundle setup documentation][vundle-qs].

[joegoggins]: https://gist.github.com/joegoggins/8482408
