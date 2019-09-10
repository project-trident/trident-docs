Author: [jdrch](https://github.com/jdrch)

Last updated: 2019-09-09

**How to set up regular recurring, incremental, online filesystem backups using `restic`**

*What this guide will get you:*

* The ability to create directory snapshots, including the root directory `/`
* Automatic snapshot pruning. Note that this guide treats backup deletion and pruning as a single prune operation, while `restic`'s documentation treats them separately
* Snapshots that contain only changes since the previous snapshot
* Block/physical storage device redundancy. Snapshots will be located on a HDD or SSD from the source data
* Online snapshots
* Redundancy against data corruption [when paired with ECC RAM](https://pthree.org/2013/12/10/zfs-administration-appendix-c-why-you-should-use-ecc-ram/)  

*What this guide will NOT get you:*

* Instantaneous snapshots. See [`zfsnap`](https://github.com/project-trident/trident-docs/blob/master/zfssnap.md) for that
* Device-level backup. For that, try [Bareos](https://www.bareos.org/en/), [Bacula](https://www.bacula.org/), or [Amanda](http://www.amanda.org/). No claim about those tools is made or implied by this statement

**STEP 0: Get the right mindset**

Be prepared to NOT understand things at first, but also be patient. Eventually you'll get the hang of what you're doing. 

Snapshots are just backups and `restic` operates on those exclusively, so there is minimal risk to your data during setup. The mistake you're most likely to make is setting `restic` to run too often. This might cause performance problems if a `restic` job is still running while another one starts.

This guide is written to enable people unfamiliar basic programming concepts and who do better with GUIs than CLIs to understand how the commands involved work. Too many Linux/Unix guides assume a lot of prior knowledge for which no easily accessible corresponding guide or documentation exists. This leaving users struggling to understand what they need to do to get their desired effects.

**STEP 1: Read the `restic` documentation**

Read the [`restic`](https://restic.readthedocs.io) documentation.

**STEP 2: Read FreeBSD's crontab and ZFS documentation**

While `restic` creates and automatically prunes backups, this guide uses `cron` to invoke it at preset intervals. As such, you'll have to be familiar with crontab syntax to create the `restic` run schedule you want. crontab syntax varies based on OS implementation, and Project Trident (ultimately) [uses FreeBSD's implementation thereof](https://t.me/ProjectTrident/33871). 

Read:

1. The [crontab man page](https://www.freebsd.org/cgi/man.cgi?crontab(5))
2. [Configuring `cron`](https://www.freebsd.org/doc/handbook/configtuning-cron.html) in the FreeBSD Handbook
3. The [`zpool` man page](https://www.freebsd.org/cgi/man.cgi?query=zpool&sektion=8&apropos=0&manpath=FreeBSD+12.0-RELEASE+and+Ports)
4. The [`zfs` man page](https://www.freebsd.org/cgi/man.cgi?query=zpool&sektion=8&apropos=0&manpath=FreeBSD+12.0-RELEASE+and+Ports)

**STEP 3: Read the ZFS Administration Guide**

ZFS is the basis for the physical storage and data corruption redundancies pointed out at the outset, so you'll need to be familiar with it. 

The [ZFS Administration Guide](https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/) was written for Debian and Ubuntu, but the same principles apply to Trident, and the author makes it clear where anything is specifc to those OSes and not necessarily applicable to others.  

It is entirely OK, acceptable, and even preferable to take a long time going through the administration guide.

**STEP 4: Ensure at least 2 extra HDDs or SSDs besides the source one are installed on the computer**

Ensure none of the extra drives contain data you need, as said data will be destroyed during zpool creation. Internal drives are preferable.

Given the wide range of possibilities for fulfulling this step, details are not provided.

**STEP 5: Create a zpool from the drives from Step 4**

This is done using the `zpool create` command. See these [examples](https://pthree.org/2012/12/04/zfs-administration-part-i-vdevs/).

It is *strongly* recommended by this author that the `autoexpand` option is enabled for the pool during or after creation. To enable it after creation in QTerminal for a zpool named *tank* - following the example in the Administration Guide - run `zpool set autoexpand=on tank`.

**STEP 6: Create a ZFS filesystem on the zpool created in Step 5**

This is done using the `zfs create` command. See these [examples](https://pthree.org/2012/12/17/zfs-administration-part-x-creating-filesystems/).

It is *strongly* recommended by this author that the `compression` option is enabled for the filesystem during or after creation. To do this in QTerminal after `zfs create` has completed, run `zfs set compression=on tank/ZFSFileSystemName`.

Enable `dedup` for the filesystem if you have [sufficient RAM](https://pthree.org/2013/12/18/zfs-administration-appendix-d-the-true-cost-of-deduplication/). To do this in QTerminal after `zfs create` has completed, run `zfs set dedup=on tank/ZFSFileSystemName`.

**STEP 7: Download and install `restic`**

1. On the Project Trident desktop, open the Lumina start menu
2. Search for *AppCafe*
3. In the window that opens, search for `restic`. There should be only one result by that exact name
4. Doubleclick the `restic` result entry
5. In the upper right of the *AppCafe* window, click *Install*
6. Click the *Pending* tab
7. Wait for the status of the installation job to go to *Finished*

**STEP 8: Determine the directory structure of your backups**

Each `restic` `backup` command specifies a source and a repository. The main things to know about repositories are:

1. You can have as many repositories as you want
2. Each repository is deduplicated on its own. So, for example, consider 2 folders with the same files backed up to a single repository. That repository would have only 1 copy of each file. However, if those same 2 folders backup to separate repositories, then each repository has its own copy
3. Each repository has its own access credentials
4. Backup pruning operates per repository
5. Multiple sources can backup to the same respository
6. Multiple repositories can backup the same source
7. Sources and repositories are not linked. You can backup a source to any existing repository you have access to by constructing and running a matching `restic` command

This guide will assume the user is backing up a single source directory to a single repository. 

**STEP 9: Initialize the repository**

Folllow these [instructions](https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html#local).

**STEP 10: Create a file containing the repository password**

1. In QTerminal, launch Lumina Text Editor as root using `sudo lte`
2. Enter the respository password. Ensure there is absolutely nothing else in the file
3. Click the Save icon
4. Name the file - this guide will call it *PasswordFilename* - and save it in the `/root` directory

**STEP 11: Ensure only root can read the repository password file**

It is important that only root can read the file so that the password isn't compromised.

1. In QTerminal, navigate to `/root` via running `cd /root`
2. Change the permissions of the password file so that only the owner can read it, by running `sudo chmod 600 PasswordFilename`
3. Change the owner of the file to root by running `sudo chown root PasswordFilename`
4. Test that you can't read the file by trying to open it from Lumina File Manager. Even if the app to read the file launches, it should just display blank content

**STEP 12: Create an environment variable for the password file**

Automated backups require the corresponding respository password without the user manually entering it. `restic` does this by [reading an environment variable whose value is the password file's path](https://restic.readthedocs.io/en/latest/faq.html?highlight=password#how-can-i-specify-encryption-passwords-automatically). Create the environment variable by:

1. In QTerminal, run `export RESTIC_PASSWORD_FILE=/root/PasswordFilename`
2. Test that the above command worked by running `$RESTIC_PASSWORD_FILE`. It should return `/root/PasswordFilename`

**STEP 13: Construct your backup command**

1. In QTerminal, run `which restic`. This will give you an absolute path to `restic`, which this guide will call `AbsolutePathToRestic`. Typically the absolute path is `/usr/local/bin/restic`
2. Run `AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository --verbose backup AbsolutePathToSource`
3. If 2) above is successful, then the backup command is the same but without the `--verbose`: `AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository backup AbsolutePathToSource`

**STEP 14: Determine the crontab schedule for your backups**

1. Think about how often you want to create backups
2. The above bulletpoint is determined entirely by crontab, so *write down* a crontab schedule that matches the above, based on the crontab syntax in 2.1 above
3. Pay attention to how long it took for the command in 13.2 to run. There should be *at least* that much time between backups to prevent job collisions

A couple details about 2.1:

* crontab fields support whole numbers only; e.g. `0 2 * * *` will work, `0 2.2 * * *` will not work
* /*n*, where *n* is a number, does not work in the **Minutes** field. Without going into details, just avoid it
* The `@`*n*`s` syntax, where *n* is a whole number, is much easier to understand than the individual fields. It ensures that each successive invocation happens *n* seconds *after the previous one has completed*. This means tasks in the same crontab entry never collide. The main drawback is it's more difficult to set jobs based on absolute calendar date and time

An example of a crontab schedule is `0	14	*	*	*` (tab separated), which translates to:

* Every time the system clock time value is 0 minutes, 14 hours (represented by `0 14`, evaluates to 14:00/2:00 PM) ...
* Regardless of the day of the month, the month, or the day of the week (what `* * *` stand for, respectively)

Or, put into one sentence: Every day at 14:00/2 PM.

**STEP 15: Match desired backups with their corresponding crontab schedules to create single, complete crontab entries for each backup, with the user the `restic` command should run as**

For example, putting the examples in Steps 13 and 14 together - in that sequence - into a sample crontab entry gives:

`0 14 * * * root AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository backup AbsolutePathToSource`

Which translates to: 

* Every time the system clock time value is 0 minutes, 14 hours (represented by `0	14`, evaluates to 14:00/2:00 PM) ...
* Regardless of the day of the month, the month, or the day of the week (what `* * *` stand for, respectively) ...
* As the user root (represented by `root`) ...
* Invoke `restic` (represented by the absolute path to the `restic` command) to ...
* Read the repository password from the password file (represented by `-p $RESTIC_PASSWORD_FILE`) and ...
* Open the repository located at `AbsolutePathToRepository` (represented by `-r AbsolutePathToRepository`) and ...
* Backup the source directory (represented by `backup AbsolutePathToSource`)
 
Or, put into one sentence: As root, open the repository using the password and backup the source to the repository every day at 14:00/2 PM.

**STEP 16: Create a backup retention policy**

This is described [here](https://restic.readthedocs.io/en/latest/060_forget.html#removing-snapshots-according-to-a-policy).

The command you come up with should look like: `AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository forget --ForgetParameters -prune`. Note the lack of reference to the source directories; this command covers *all* the contents of a repository regardless of source.

**STEP 17: Determine the crontab schedule for your backup pruning**

This is similar to Step 14, while ensuring that backups and prunes don't occur simultaneously. 

**STEP 18: Match desired prunes with their corresponding crontab schedules to create single, complete crontab entries for each prune, with the user the `restic` command should run as**

An example of a combined command for the above is:

`@100000 root AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository forget --keep-last 7 -prune`

Which translates to: 

* 100000s after the completion of the previous invocation of the following command by `cron` (represented by `@100000s`) ...
* As the user root (represented by `root`) ...
* Invoke `restic` (represented by the absolute path to the `restic` command) to ...
* Read the repository password from the password file (represented by `-p $RESTIC_PASSWORD_FILE`) and ...
* Open the repository located at `AbsolutePathToRepository` (represented by `-r AbsolutePathToRepository`) and ...
* Forget backups (represented by `forget`) that meet the criteria of ...
* Keeping the most recent 7 backups (represented by `--keep-last 7`) and ...
* Pruning the data associated with them (represented by `prune`)

Or, put into a single sentence: As root, open the repository using the password and delete all backups made more than 100000 seconds after the completion of the previous invocation and their associated data that except the 7 most recent backups.

**STEP 19: Put each combined `cron` + `restic` command into `/etc/crontab`**

1. Open crontab in Lumina Text Editor via `sudo lte /etc/crontab`. Unlike on (some) Linux distros, crontab can be edited directly in a GUI application on Project Trident.
2. Put each combined command into the crontab file, with each line preceded by a comment describing what the line does. This is helpful for troubleshooting
3. Save the crontab file and exit Lumina Text Editor

The additional crontab entries should look like this:

```
# Create a backup of source directory daily at 2 PM

0 14 * * * root AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository backup AbsolutePathToSource
```

```
# Delete all backups and associated data from repository except for the most recent 7

@100000 root AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository forget --keep-last 7 -prune
```
And that's it!
