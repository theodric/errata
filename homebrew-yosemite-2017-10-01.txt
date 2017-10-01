As of 2017-10-01 there seems to be some strange issue with either the latest patchlevel of Yosemite (14F2511), 
Homebrew, or the combination. This affected both my existing, previously-working installation of Homebrew, 
as well as an attempt at a cleanup followed by a fresh install.

The error will manifest as follows:

~$ brew update
Initialized empty Git repository in /usr/local/Homebrew/.git/
remote: Counting objects: 1124, done.
remote: Compressing objects: 100% (1033/1033), done.
remote: Total 1124 (delta 116), reused 377 (delta 49), pack-reused 0
Receiving objects: 100% (1124/1124), 1.21 MiB | 0 bytes/s, done.
Resolving deltas: 100% (116/116), done.
From https://github.com/Homebrew/brew
 * [new branch]      master     -> origin/master
HEAD is now at 533ea9d Merge pull request #3252 from MikeMcQuaid/no-export-default-make
To restore the stashed changes to /usr/local/Homebrew run:
  'cd /usr/local/Homebrew && git stash pop'
==> Downloading https://homebrew.bintray.com/bottles-portable/portable-ruby-2.3.3.leopard_64.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring portable-ruby-2.3.3.leopard_64.bottle.1.tar.gz
compress: /dev/stdin: Inappropriate file type or format
child returned status 1
no data in archive
Error: Failed to install vendor ruby.
Error: Failed to install vendor Ruby.

Homebrew seems to be calling 'compress' to unpack a gzipped tarball. This strikes me as a developer-induced error, 
and since it's failing with a mismatched archive type, I think I'm right.

Rather than trying to figure out where (in the impenetrable thicket of scripts that is Homebrew) 
the problem lay, I took a brute-force approach:

cd /usr/bin
mv compress compress.dis
ln -s gunzip compress

I then cleaned up and ran the Homebrew installation script again, which succeeded, albeit with noisy errors.
Homebrew is nevertheless fully functional.

$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
==> This script will install:
/usr/local/bin/brew
/usr/local/share/doc/homebrew
/usr/local/share/man/man1/brew.1
/usr/local/share/zsh/site-functions/_brew
/usr/local/etc/bash_completion.d/brew
/usr/local/Homebrew
==> The following new directories will be created:
/usr/local/Cellar
/usr/local/Homebrew
/usr/local/Frameworks
/usr/local/opt
/usr/local/sbin

Press RETURN to continue or any other key to abort
==> /usr/bin/sudo /bin/mkdir -p /usr/local/Cellar /usr/local/Homebrew /usr/local/Frameworks /usr/local/opt /usr/local/sbin
==> /usr/bin/sudo /bin/chmod g+rwx /usr/local/Cellar /usr/local/Homebrew /usr/local/Frameworks /usr/local/opt /usr/local/sbin
==> /usr/bin/sudo /bin/chmod 755 /usr/local/share/zsh /usr/local/share/zsh/site-functions
==> /usr/bin/sudo /usr/sbin/chown theodric /usr/local/Cellar /usr/local/Homebrew /usr/local/Frameworks /usr/local/opt /usr/local/sbin
==> /usr/bin/sudo /usr/bin/chgrp admin /usr/local/Cellar /usr/local/Homebrew /usr/local/Frameworks /usr/local/opt /usr/local/sbin
==> /usr/bin/sudo /bin/mkdir -p /Users/theodric/Library/Caches/Homebrew
==> /usr/bin/sudo /bin/chmod g+rwx /Users/theodric/Library/Caches/Homebrew
==> /usr/bin/sudo /usr/sbin/chown theodric /Users/theodric/Library/Caches/Homebrew
==> /usr/bin/sudo /bin/mkdir -p /Library/Caches/Homebrew
==> /usr/bin/sudo /bin/chmod g+rwx /Library/Caches/Homebrew
==> /usr/bin/sudo /usr/sbin/chown theodric /Library/Caches/Homebrew
==> Downloading and installing Homebrew...
remote: Counting objects: 91449, done.
remote: Total 91449 (delta 0), reused 1 (delta 0), pack-reused 91448
Receiving objects: 100% (91449/91449), 21.17 MiB | 7.51 MiB/s, done.
Resolving deltas: 100% (66202/66202), done.
From https://github.com/Homebrew/brew
 * [new branch]      master     -> origin/master
 * [new tag]         0.1        -> 0.1
 * [new tag]         0.2        -> 0.2
 * [new tag]         0.3        -> 0.3
 * [new tag]         0.4        -> 0.4
 * [new tag]         0.5        -> 0.5
 * [new tag]         0.6        -> 0.6
 * [new tag]         0.7        -> 0.7
 * [new tag]         0.7.1      -> 0.7.1
 * [new tag]         0.8        -> 0.8
 * [new tag]         0.8.1      -> 0.8.1
 * [new tag]         0.9        -> 0.9
 * [new tag]         0.9.1      -> 0.9.1
 * [new tag]         0.9.2      -> 0.9.2
 * [new tag]         0.9.3      -> 0.9.3
 * [new tag]         0.9.4      -> 0.9.4
 * [new tag]         0.9.5      -> 0.9.5
 * [new tag]         0.9.8      -> 0.9.8
 * [new tag]         0.9.9      -> 0.9.9
 * [new tag]         1.0.0      -> 1.0.0
 * [new tag]         1.0.1      -> 1.0.1
 * [new tag]         1.0.2      -> 1.0.2
 * [new tag]         1.0.3      -> 1.0.3
 * [new tag]         1.0.4      -> 1.0.4
 * [new tag]         1.0.5      -> 1.0.5
 * [new tag]         1.0.6      -> 1.0.6
 * [new tag]         1.0.7      -> 1.0.7
 * [new tag]         1.0.8      -> 1.0.8
 * [new tag]         1.0.9      -> 1.0.9
 * [new tag]         1.1.0      -> 1.1.0
 * [new tag]         1.1.1      -> 1.1.1
 * [new tag]         1.1.10     -> 1.1.10
 * [new tag]         1.1.11     -> 1.1.11
 * [new tag]         1.1.12     -> 1.1.12
 * [new tag]         1.1.13     -> 1.1.13
 * [new tag]         1.1.2      -> 1.1.2
 * [new tag]         1.1.3      -> 1.1.3
 * [new tag]         1.1.4      -> 1.1.4
 * [new tag]         1.1.5      -> 1.1.5
 * [new tag]         1.1.6      -> 1.1.6
 * [new tag]         1.1.7      -> 1.1.7
 * [new tag]         1.1.8      -> 1.1.8
 * [new tag]         1.1.9      -> 1.1.9
 * [new tag]         1.2.0      -> 1.2.0
 * [new tag]         1.2.1      -> 1.2.1
 * [new tag]         1.2.2      -> 1.2.2
 * [new tag]         1.2.3      -> 1.2.3
 * [new tag]         1.2.4      -> 1.2.4
 * [new tag]         1.2.5      -> 1.2.5
 * [new tag]         1.2.6      -> 1.2.6
 * [new tag]         1.3.0      -> 1.3.0
 * [new tag]         1.3.1      -> 1.3.1
 * [new tag]         1.3.2      -> 1.3.2
 * [new tag]         1.3.3      -> 1.3.3
 * [new tag]         1.3.4      -> 1.3.4
HEAD is now at 533ea9d Merge pull request #3252 from MikeMcQuaid/no-export-default-make
==> Downloading https://homebrew.bintray.com/bottles-portable/portable-ruby-2.3.3.leopard_64.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring portable-ruby-2.3.3.leopard_64.bottle.1.tar.gz
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Could not create file portable-ruby/2.3.3/lib/ruby/gems/2.3.0/gems/did_you_mean-1.0.0/lib/did_you_mean/spell_checkers/nam: Is a directory
Read access to ././@LongLink was denied
Could not create file portable-ruby/2.3.3/lib/ruby/gems/2.3.0/gems/did_you_mean-1.0.0/lib/did_you_mean/spell_checkers/nam: Is a directory
Read access to ././@LongLink was denied
Could not create file portable-ruby/2.3.3/lib/ruby/gems/2.3.0/gems/did_you_mean-1.0.0/lib/did_you_mean/spell_checkers/nam: Is a directory
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
Read access to ././@LongLink was denied
==> Tapping homebrew/core
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...
remote: Counting objects: 4596, done.
remote: Compressing objects: 100% (4383/4383), done.
remote: Total 4596 (delta 28), reused 344 (delta 6), pack-reused 0
Receiving objects: 100% (4596/4596), 3.83 MiB | 6.85 MiB/s, done.
Resolving deltas: 100% (28/28), done.
Checking connectivity... done.
Tapped 4374 formulae (4,642 files, 11.9MB)
==> Cleaning up /Library/Caches/Homebrew...
==> Migrating /Library/Caches/Homebrew to /Users/theodric/Library/Caches/Homebrew...
==> Deleting /Library/Caches/Homebrew...
Already up-to-date.
==> Installation successful!

==> Homebrew has enabled anonymous aggregate user behaviour analytics.
Read the analytics documentation (and how to opt-out) here:
  https://docs.brew.sh/Analytics.html

==> Next steps:
- Run `brew help` to get started
- Further documentation:
    https://docs.brew.sh

I would suggest undoing the changes to 'compress' once you get Homebrew installed.
