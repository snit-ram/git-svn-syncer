# Git svn syncer

These scripts are intend to be used to sync an existing git repository into a
(preferable empty) svn repo via cron job, by using an interstitial git-svn clone.

The script works well on pre-populated svn repositories, but it's hard
recommended that you use an empty svn repository.


## first-sync

This script is responsible for fetching both git and svn commits and to do the
first git to svn sync. You should run this script first.

It assumes that your first git commit starts after your last svn commit.

Usage:

    $ ./first-sync <svn-url> <git-url> <gitsvn-clone-path>


* `svn-url` is the checkout url of your svn repository, without the `trunk` part.
Example: https://scribus.svn.sourceforge.net/svnroot/scribus

Example: git@github.com:snit-ram/git-svn-syncer.git

* `gitsvn-clone-path` is the path where first-sync will create the interstitial
git-svn checkout to manage your syncs. Example: ~/git-svn-syncs/myproject/



## cron-sync

This script is inteded to be used as a cron job to keep syncing git into svn
repositories afer the first sync is done.

Usage:

    $ ./cron-sync <gitsvn-clone-path>


* `gitsvn-clone-path` this is the same path used in `first-sync` script.



### Cron sync notifications

Crontab provides mail notifications for running jobs. You can use this feature
to receive eventual failure notifications for your `cron-sync`.

Below is a sample crontab script that syncs git and svn every minute, and
notificates me by mail when `cron-sync` fails.

    MAILTO=snit.ram@gmail.com

    * * * * * /home/git/git-svn-syncer/cron-sync /home/git/git_syncer_repositories/test-repo/ 1>/dev/null

