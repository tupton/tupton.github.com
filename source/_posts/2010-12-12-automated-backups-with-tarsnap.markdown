---
date: '2010-12-12 20:14:08'
layout: post
slug: automated-backups-with-tarsnap
status: publish
title: Automated Backups with Tarsnap
wordpress_id: '605'
categories:
- Articles
- Code
- Howto
- Software
---

I remember [reading][ta1] [about][ta2] [Tarsnap][t] a couple of years ago, back when it was only an idea. I wasn't too convinced about using a service that was in beta to back up my data, but I recently rediscovered that it had graduated to a full-blown product and signed up immediately.

[t]: http://www.tarsnap.com/
[ta1]: http://www.daemonology.net/blog/2006-09-13-encrypted-backup.html
[ta2]: http://www.daemonology.net/blog/2008-11-10-tarsnap-public-beta.html

Tarsnap is an encrypted backup tool based on archives. I'm not going to go into any details about the implementation, but you can read about the [cryptography][tc], [the security][ts], or anything else about the [overall design][td] of the tool on the [Tarsnap site][t]. Basically, it creates archives (hence the "tar" part of the name), encrypts them, and stores them on [Amazon S3][s3]. The "snap" part of the name refers to the idea that backups are done in "snapshots," which means that backups are incremental and duplicate data can be shared between archives.

[td]: http://www.tarsnap.com/design.html
[tc]: http://www.tarsnap.com/crypto.html
[ts]: http://www.tarsnap.com/security.html
[s3]: http://aws.amazon.com/s3/

After you sign up for a Tarsnap account, put at least $5 (via Paypal) into your account, and [generate a key][tk], you can begin backing up your data. You can read more about [getting started][tgs] and [using `tarsnap` in general][tu], but I really want to talk about automated backups with Tarsnap.

[tk]: http://www.tarsnap.com/man-tarsnap-keygen.1.html
[tgs]: http://www.tarsnap.com/gettingstarted.html
[tu]: http://www.tarsnap.com/usage.html

## A Simple Wrapper

I found [a blog post by Jonathan Street][js] that detailed his automated backups, and that served as inspiration for my system. I wrote a little bash script to wrap `tarsnap` for my purposes:

``` bash
    #! /bin/bash
    echo `date +%F\ %T`: Beginning back up of $2
    /usr/local/bin/tarsnap -c -f $1-`date +%F` $2
    echo `date +%F\ %T`: Completed back up of $2
```

Calling `tarsnap-backup.sh  ` tells tarsnap to create an archive  of the specified directory with the given name and the current date. I was in business.

[js]: http://jonathanstreet.com/blog/setting-up-backups-with-tarsnap

### Generating a new key

An aside: Jonathan Street's blog post mentioned creating a new key that only had permission to read and write archives. I initially did the same thing, but for reasons I'll get into later, I wanted the ability to delete backups, too. Generating a new key was extremely easy:

``` bash
    $ tarsnap-keymgmt --outkeyfile /root/tarsnap-rw.key -r -w /root/tarsnap.key
```

This creates a new key in `/root/` called `tarsnap-rw.key` that only has read and write permission.

## Automation

### `newsyslog`

The simple wrapper script above was great, but if I was going to automate it, I needed those `echo` statements to go to a more permanent log file. If I was going to do daily backups of directories, I needed some sort of log management. After searching around a bit, it became clear that `newsyslog` was the way to go on OS X. Looking at the file in `/etc/newsyslog.conf` was enough to give me the basic file structure, but the [man pages][nsl] go into a lot of detail.

[nsl]: http://www.openbsd.org/cgi-bin/man.cgi?query=newsyslog&sektion;=8

I made a configuration called `user.conf` in `/etc/newsyslog.d/` and put my tarsnap logs inside. I decided to use a distinct log for each automated backup I do, as opposed to a single tarsnap log. I still haven't decided if this is the right way to go, but I do like being able to quickly see the result of the last backup. My `user.conf` looks like the following.

    /var/log/tarsnap-backup-code.log                        640     5       1000    *       Z
    /var/log/tarsnap-backup-documents.log                   640     5       1000    *       Z

This configuration tells `newsyslog` to gzip, roll to a new log once the current log exceeds 1MB in size, and keep at most five old logs.

### `cron`

With log rotation in place, I could create a cron job.

    0 4 * * * /usr/local/bin/tarsnap-backup code ~/code > /var/log/tarsnap-backup-code.log

This crontab schedules backups for my `code` directory at 4am daily and my `Documents` directory at 5am daily. I used `sudo crontabe -e` to create this because both `tarsnap` and my log file's permissions require root privileges. This would have sufficed, but there was a nagging thought in the back of my head: I [knew][w] that `launchd` is used in place of `cron` in OS X, and I thought this would give me a good opportunity to dive into even more options that `launchd` has to offer.

[w]: http://www.thomasupton.com/blog/2009/09/i-love-weather/

### `launchd`

Since I wanted these backups to run whenever possible, I decided to put my `launchd` backup configurations in `/Library/LaunchDameons` instead of `/Library/LaunchAgents`. LaunchDaemons are able to run without a logged-in user; this is exactly what I wanted. The `launchd` configuration for my `code` backup looks like the following:

``` html
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN"
    "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.thomasupton.backup-daily-code</string>
        <key>ProgramArguments</key>
        <array>
            <string>/usr/local/bin/tarsnap-backup</string>
            <string>code</string>
            <string>/Users/thomas/code</string>
        </array>
        <key>GroupName</key>
        <string>wheel</string>
        <key>UserName</key>
        <string>root</string>
        <key>Nice</key>
        <integer>1</integer>
        <key>StandardErrorPath</key>
        <string>/var/log/tarsnap-backup-code.log</string>
        <key>StandardOutPath</key>
        <string>/var/log/tarsnap-backup-code.log</string>
        <key>StartCalendarInterval</key>
        <dict>
            <key>Hour</key>
            <integer>5</integer>
            <key>Minute</key>
            <integer>0</integer>
        </dict>
    </dict>
    </plist>
```

The `ProgramArguments` section is exactly how I called the backup script from `cron`. The `UserName` and `GroupName` keys are important: they tell `launchd` to run the backup script as root, which, as I mentioned before, is necessary for using `tarsnap` and for appending to the log file. The `StandardErrorPath` and `StandardOutPath` keys tell `launchd` to redirect output to the proper log file. The `StartCalendarInterval` tells `launchd` to run this script at 5am daily.

After registering the configuration via `launchctl load /Library/LaunchDaemons/com.thomasupton.backup-daily-documents.plist`, my automated backup system was in place.

## Backup Management

Since Tarsnap backs up data with the notion of "snapshots" and keeps track of blocks of data (and not archive data), keeping multiple archives of the same data doesn't make much sense. However, running a daily backup by creating a new archive would mean that many archives would build up fast. I decided that keeping at most three previous backups of the same data would suffice. I wanted to automate this, too. This is the reason I decided not to use a read-write-only key.

I added the following lines to my `tarsnap-backup.sh` script.

``` bash
    # Remove the backup from three days previous, if there is one
    echo `date +%F\ %T`: Removing backup of $2 from `date -v-3d +%F`
    /usr/local/bin/tarsnap -d -f $1-`date -v-3d +%F`
    echo `date +%F\ %T`: Completed removing backup of $2 from `date -v-3d +%F`
```

The key to this is the date in the archive name passed to `tarsnap -d`. `date -v` lets you add a value to the date output, so `-v-3d` outputs the date from three days previous. Now, every scheduled backup attempts to delete the archive from three days ago in addition to creating a backup for the current day. Of course, if a backup is missed, this can lead to an accumulation of old archives. This is where the log files come in handy: I can just inspect the logs every couple of days to see what successfully ran and manually prune the archive list if necessary.

### Large Backups

I said "if a backup is missed," but I didn't mention why that might occur. The answer becomes apparent when you start talking about backing up large amounts of data. My `~/Documents` folder was over 12GB, and with my terrible upload speeds, that would mean that it would take a long, long time to upload everything. Even though I was able to prune the contents of `~/Documents` down to 6.5GB, I still needed more than an hour to back it up. `tarsnap` doesn't perform more than one archive transaction at once, so if the `documents` archive was still running when the `code` archive process began, tarsnap would cancel the latter and continue with the former, hence a backup is missed. This is also another reason that I decided to keep separate log files for each backup job. The log lines for an in-progress job aren't interspersed with a failed attempt to start another backup job.

The `documents` backup was still too large to have been done by the morning, and I didn't really want to sacrifice my network connection just for the sake of a backup. Fortunately, `tarsnap` supports archive truncation. According to the [man pages][tm], `tarsnap` responds to the `SIGQUIT` interrupt by truncating the archive and appending "`.part`" to the archive name. When my large backup job was still running, all I had to do was send the `SIGQUIT` signal with `kill -3` (alternatively, you could send `^Q` if you use `tarsnap` from a console and not from a scheduled job) and `tarsnap` would effectively "pause" the backup. The next time that same data is archived, `tarsnap` will recognize it and only upload new data. This works even with a different archive name, thanks to snapshots and block data.

[tm]: http://www.tarsnap.com/man-tarsnap.1.html

## Restoring Backups

Tarsnap is a great service, but truly for those who know what they are doing. It took me far longer than I would like to admit to come up with a process for all of this, but it was worth it. Of course, creating backups is only one part of a complete system. The other, more important part, is restoration. Since `tarsnap` is built on `tar` and `libarchive`, this is incredibly simple. `tarsnap -x` extracts archives, and `tarsnap -r` writes a tar stream to `stdout`, which can be used to create a local tar.

If you like the idea of easy, encrypted backups, tarsnap is a great service. It's cheap, secure, and reliable, plus it's fun and easy to use if you're comfortable with UNIX-style archiving tools.
