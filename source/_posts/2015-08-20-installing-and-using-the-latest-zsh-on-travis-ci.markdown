---
layout: post
title: "Installing the Latest ZSH on Travis CI"
date: 2015-08-20 20:25:07 -0500
comments: true
categories:
- Howto
- Software
---

I recently thought it might be a good idea to start using [Travis CI][travis] to run builds of my personal repositories on a regular basis. A lot of my repositories are pet projects, but that doesn't mean that I don't depend on them on a daily basis.

  [travis]: https://travis-ci.org/

That couldn't be more true of [my dotfiles][df]. My [`zshrc`][zshrc] and my [`vimrc`][vimrc] get exercised tens if not hundreds of times per day. Sometimes I'll make a change to test out something new, verify that it doesn't blow up, commit it, and move on. That's probably not the best way to do things, but I figure that I'll never start using the new hotness if I don't jump in and start exercising it right away. Usually this works out well, but it can potentially break my environment in subtle ways. Continuous integration can help with that: if I commit a breaking change, I can get an email when the "build" breaks. I'll immediately know which commit broke something without having to resort to `git blame` or something similar.

  [df]: https://github.com/tupton/dotfiles
  [zshrc]: https://github.com/tupton/dotfiles/blob/a5597784778bca973188300b8fff40f9688b2cf5/zsh/zshrc
  [vimrc]: https://github.com/tupton/dotfiles/blob/a5597784778bca973188300b8fff40f9688b2cf5/vim/vimrc

Travis CI offers a fantastic free service, but I haven't really had a chance to use it yet[^travis-vs-jenkins]. I figured that setting up CI for my small dotfiles repository would be a great way to learn.

  [^travis-vs-jenkins]: We use Jenkins at work, which is a blessing and a curse. Mostly the latter, but the sheer number of plugins available coupled with the fact that I work with some awesome people who know Jenkins better than I ever will makes it ok.

There was one huge hurdle: I use `zsh`, and most of my dotfile setup scripts are written in `zsh`, but the Travis environment only comes with the `bash` shell installed.

Some [existing open source projects][zsh-travis-old] use Travis with `zsh`, but they all use the legacy environment that still allows `sudo`, not the [newer container-based environment][travis-container]. The [apt addon][travis-apt] can help install packages in containers, but the latest version of `zsh` on Ubuntu 12.04 is 4.3.17. `zsh` 5 is a requirement for most modern usages, so that's a non-starter. I thought that someone would have come across this already and solved it, and maybe they have, but I couldn't easily find a solution.

  [zsh-travis-old]: https://github.com/zsh-users/antigen/blob/7860ce7aecdbed8fd8ff75472ac59c52c2ac9a7e/.travis.yml#L32
  [travis-container]: http://docs.travis-ci.com/user/migrating-from-legacy/
  [travis-apt]: http://docs.travis-ci.com/user/apt/

We need to build and install `zsh`, and we need to do it without sudo. `build-essential` is already available on the Travis CI virtual machines, and we could use the aforementioned apt addon if it wasn't.

After much [trial and error][dotfile-build-history], I finally got a Travis config that makes sure a recent version of zsh is set up before running the build. I chose to do this in the `before_install` step because that seems to be where additional dependencies should be installed, but I suppose it could be done anywhere in the build lifecycle before `script` runs the actual tests.

  [dotfile-build-history]: https://travis-ci.org/tupton/dotfiles/builds

The full Travis config follows, but the `before_install` step is what really matters:

``` yaml
language: zsh
addons:
  apt:
    packages:
    - build-essential
before_install:
- export LOCAL="$(mktemp --directory --tmpdir=${TMPDIR:/tmp} local.bin.XXXXXX)"
- curl -L http://downloads.sourceforge.net/zsh/zsh-5.0.7.tar.gz | tar zx
- cd zsh-5.0.7
- ./configure --prefix=$LOCAL
- make
- make install
- cd -
- export PATH="$LOCAL/bin:$PATH"
script: make test
```

First, we make a temporary directory to install zsh to. Remember, no `sudo` means no access to `/usr/bin/local`, so we need to choose a safe location to install.

Next, we download the latest version of `zsh`. If you want to use a different mirror or download an archive with a different compression, you just need to change this line and rest should work.

Then we build `zsh`: configure with the prefix set to the directory we created earlier, then `make` it and `make install` it to the prefixed directory.

Finally, we `cd` back to the directory we were in – just in case – and, more importantly, we add the temprorary directory to the beginning of our path so that `zsh` can be found.

These `before_install` steps could probably be extracted into a script, but I wanted to go with the simplest Travis config with the least overhead to get up and running with `zsh`. Now, when the test script runs, `zsh` is available and we can check our scripts for errors!

I look forward to exploring more of what is possible with Travis. On my horizon: using [`vint`][vint] to lint my vimrc and writing tests for the majority of my private repositories that don't currently have any verification.

  [vint]: https://github.com/Kuniwak/vint
