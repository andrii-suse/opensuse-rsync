# openSUSE-rsync
-----------------------

openSUSE-rsync will generate rsync commands to sync openSUSE repositories

## Motivation

openSUSE infrastructure provides rsync modules, which are big and contain both dynamic and stale files sets.
download.opensuse.org has concept of projects available in /app/project, which might be useful abstraction for managing content of mirror instead of current rsync modules.


The purpose of opensuse-rsync:
- Let user choose which projects they want to sync and estimate approximate total disk usage;
- Preview and customize generated rsync commands;
- Using api on download.opensuse.org skip the sync whatsoever if no files were changed in the project;
- Use named locks and log files for each project;
- Use cache file for each project to skip sync if api shows nothing was published;
- Potentially define a common way for syncing openSUSE mirrors and intergrate with MirrorCache.

## Synopsis - preview commands

```
# choose which projects to sync
echo PROJECT_SLOWROLL_ISO=1 >> /etc/opensuse-rsync.env
echo PROJECT_TUMBLEWEED_ISO=0 >> /etc/opensuse-rsync.env
echo PROJECT_LEAP_156_UPDATE=1 >> /etc/opensuse-rsync.env

# choose US mirror to sync from instead of default one
echo RSYNC_ADDRESS=rsync://provo-mirror.opensuse.org/opensuse/ >> /etc/opensuse-rsync.env

# preview the generated commands for tw-iso project
opensuse-rsync-print-project tw-iso

# preview the generated commands for tw-repo project with dest folder ~/srv and additional rsync parameter
OPENSUSE_RSYNC_EXTRA_PARAMS='--max-size=4k' opensuse-rsync-print-project tw-iso ~/srv

# preview the generated commands for syncing all projects as defined in the config file
OPENSUSE_RSYNC_EXTRA_PARAMS='--max-size=4k' opensuse-rsync-print ~/srv
```

## Synopsis - execute commands

```

# use user "mirror" to execute the generated commands
OPENSUSE_RSYNC_EXTRA_PARAMS='--max-size=4k' opensuse-rsync-print /srv/opensuse | sudo -eu mirror bash

```

## Project status

2024 Aug - initial alpha version, waiting for feedback

## How to report issues or ask questions:

Write an email to andrii.nikitin on domain suse.com or use the issue tracker at https://github.com/andrii-suse/opensuse-rsync/
