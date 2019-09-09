Author: [jdrch](https://github.com/jdrch)

Last updated: 2019-09-08

**How to set up regular recurring, incremental, online filesystem backups using `restic`**

*What this guide will get you:*

* The ability to create directory snapshots (including the root directory, /)
* Automatic snapshot pruning (deletion when they reach a certain age, set by the user)
* Snapshots that contain only changes since the previous snapshot
* Block/physical storage device redundancy. Snapshots will be located on a different physical block device (HDD or SSD, for the purposes of this guide) from the source data
* Snapshots that happen while the computer is on and the source directories are (part of a) mounted (filesystem)
* ([When paired with ECC RAM](https://pthree.org/2013/12/10/zfs-administration-appendix-c-why-you-should-use-ecc-ram/)) Redundancy against data corruption 

*What this guide will NOT get you:*

* Instantaneous snapshots
* Device (entire PC, including bootloader, etc.) backup. For that, try [Bareos](https://www.bareos.org/en/), [Bacula](https://www.bacula.org/), or [Amanda](http://www.amanda.org/). No claim about those tools is made or implied by this statement

**STEP 0: Get the right mindset**

Be prepared to NOT understand things at first, but also be patient. Eventually you'll get the hang of what you're doing. 

Also, because snapshots are just backups, and `restic` operates on those exclusively, there is minimal risk to your data during setup. The mistake you're most likely to make is setting `restic` to run too often, which might slow down the machine until the problem is discovered and rectified.

Advanced users may find this guide a bit like spoonfeeding, but it's deliberately written in this manner to enable people unfamiliar basic programming concepts and who do better with GUIs than CLIs to understand how crontab and the commands it calls work. Too many Linux/Unix guides assume a lot of prior knowledge for which no easily accessible corresponding guide or documentation exists, leaving users struggling to understand what they need to do to get their desired effects.

There are many links and citations to allow users to do as much (or as little) background reading as they want or need.

**STEP 1: Read the `restic` documentation**

Read the [`restic`](https://restic.readthedocs.io) documentation. As with most Linux/Unix documentation, this may be pretty dense reading, but it's the official, canonical description of the functionality this guide uses, so patiently go through it.

**STEP 2: Read FreeBSD's crontab and ZFS documentation**

While `restic` creates, names, and automatically prunes backups, it uses `cron` to invoke it automatically. As such, you'll have to be familiar with crontab syntax to create the zfsnap run schedule you want. crontab syntax varies based on OS implementation, and Project Trident (ultimately) [uses FreeBSD's implementation thereof](https://t.me/ProjectTrident/33871). 

Read:

1. The [crontab man page](https://www.freebsd.org/cgi/man.cgi?crontab(5))
2. [Configuring `cron`](https://www.freebsd.org/doc/handbook/configtuning-cron.html) in the FreeBSD Handbook
3. The [`zpool` man page](https://www.freebsd.org/cgi/man.cgi?query=zpool&sektion=8&apropos=0&manpath=FreeBSD+12.0-RELEASE+and+Ports)
4. The [`zfs` man page](https://www.freebsd.org/cgi/man.cgi?query=zpool&sektion=8&apropos=0&manpath=FreeBSD+12.0-RELEASE+and+Ports)

**STEP 3: Read the ZFS Administration Guide**

ZFS is the basis for the physical storage and data corruption redundancies pointed out at the outset, so you'll need to be familiar with it. 

The [ZFS Administration Guide](https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/) was written for Debian and Ubuntu, but the same principles apply to Trident, and the author makes it clear where anything is specifc to those OSes and not necessarily applicable to others.  

It is entirely OK, acceptable, and even preferable to take a long time going through the administration guide.

**STEP 4: Ensure at least 2 extra block storage devices besides the block storage device are installed and running on the computer**

In lay terms, ensure you have at least 2 extra HDDs or SSDs *besides* the one the source folder(s) is(are) currently installed. Ensure none of the extra drives contain data you need, as said data will be destroyed during zpool creation. Internal drives are preferable.

Given the wide range of possibilities for fulfulling this step, details are not provided.

**STEP 5: Create a zpool from the extra block devices**

This is done using the `zpool create` command. Given the range of possibilities and the extensive [examples](https://pthree.org/2012/12/04/zfs-administration-part-i-vdevs/) provided by the ZFS Administration Guide, examples are not included here.

However, it is *strongly* recommended by this author that the `autoexpand` option is enabled for the pool during or after creation. To enable it after creation in QTerminal for a zpool named *tank* (following the example in the Administration Guide), run `zpool set autoexpand=on tank`.

**STEP 6: Create a ZFS filesystem on the zpool created in Step 5**

This is done using the `zfs create` command. Given the range of possibilities and the extensive [examples](https://pthree.org/2012/12/17/zfs-administration-part-x-creating-filesystems/) provided by the ZFS Administration Guide, examples are not included here.

However, it is *strongly* recommended by this author that the `compression` and `dedup` (enable the latter *ONLY* if you have [sufficient RAM](https://pthree.org/2013/12/18/zfs-administration-appendix-d-the-true-cost-of-deduplication/)) options are enabled for the filesystem during or after creation. To enable them after creation in QTerminal for a ZFS filesystem located at `tank/backup`, run `zfs set compression=on,dedup=on tank/backup`. To enable `compression` only in QTerminal after creation for the same ZFS filesystemrun `zfs set compression=on tank/backup`.

**STEP 7: Download and install `restic`**

1. On the Project Trident desktop, open the Lumina start menu
2. Search for *AppCafe*
3. In the window that opens, search for `restic`. There should be only one result by that exact name
4. Doubleclick the `restic` result entry
5. In the upper right of the *AppCafe* window, click *Install*
6. Click the *Pending* tab
7. Wait for the status of the installation job to go to *Finished*

**STEP 8: Determine the directory structure of your backups**

Each `restic` backup *command* specifies a source and a repository. The main things to know about repositories are:

1. They are deduplicated *within* each repsository, and *not across* repositories. So, for example, let's say you have 2 folders with the same files backing up a single repository. That repository would have only 1 copy of each file. However, if those same 2 folders backup to separate repositories, then each repository has its own copy
2. Each repository has its own access credentials
3. Backup pruning operates per repository
4. Multiple sources can backup to the same (or multiple) respositories and vice versa
5. Sources and repositories are not linked. You can backup a source to any existing repository you have access to without changing any settings within `restic` itself

For the sake of simplicity, this guide will assume the user is backing up a single source directory to a repository. However, the user should not take this as a suggestion; clearly, for maxmimum space efficiency, it's best for as many sources to backup to the same repository as possible. This, however, would mean that all said sources might share the same backup credentials, which may be undesirable for security and/or privacy. There are many options here, and the user would do well to carefully consider their needs and then devise a configuration that meets said needs.

**STEP 9: Initialize the repository**

This is described [here](https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html#local).

**STEP 10: Create a file containing the repository password**

1. In QTerminal, launch Lumina Text Editor as root using `sudo lte`
2. Enter the respository password
3. Click the **Save** icon
4. Give the file a name - we'll call it *PasswordFilename* here - and save it in the `/root` directory

**STEP 11: Ensure only root can read the repository password file**

Because anyone with access to this file will also have access to the repository, it is important that only root can read the file. 

1. In QTerminal, navigate to `/root` via running `cd /root`
2. Change the permissions of the password file so that only the owner can read it, by running `sudo chmod 600 PasswordFilename`
3. Change the owner of the file to root by running `sudo chown root PasswordFilename`
4. Test that you can't read the file by trying to open it from Lumina File Manager. Even if the app to read the file launches, it should just display blank content

**STEP 12: Create an environment variable for the password file**

Automated backups will require the password, but with no user around to enter it, the backup command needs some way to access said password, contained in the password file. It does this by [reading an environment variable whose value is the password file's path](https://restic.readthedocs.io/en/latest/faq.html?highlight=password#how-can-i-specify-encryption-passwords-automatically). Create the environment variable via:

1. In QTerminal, run `export RESTIC_PASSWORD_FILE=/root/PasswordFilename`
2. Test that the above command worked by running `$RESTIC_PASSWORD_FILE`. It should return `/root/PasswordFilename`

**STEP 13: Construct your backup command**

1. In QTerminal, run `which restic`. This will give you an absolute path to `restic`, which we'll call `AbsolutePathToRestic`. Typically it's `/usr/local/bin/restic`
2. Run `AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository --verbose backup AbsolutePathToSource`
3. If 2) above is successful, then the backup command is the command in 2) without the `--verbose`: `AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository backup AbsolutePathToSource`

**STEP 14: Determine the crontab schedule for your backups**

1. Think about how often you want to create backups
2. The above bulletpoint is determined entirely by crontab, so *write down* a crontab schedule that matches the above, based on the crontab syntax in 2.1 above
3. Pay attention to how long it took for the command in 13.2 to run. There should be *at least* this much time between backups to prevent collisions

A couple details about 2.1:

* crontab fields support whole numbers only, e.g. `0 2 * * *` will work, `0 2.2 * * *` will not work
* /*n*, where *n* is a whole number, e.g. `0/5`, does not work (the way you might think) for the Minutes field. Without going into details, just avoid it
* The `@`*n*`s` syntax, where *n* is a whole number, e.g. `@1000s`, is much easier to understand than the individual fields. In addition, it ensures that each successive invocation happens *n* seconds *after the previous one has completed*, which means tasks in the same family (crontab entry) never collide (read: attempt to start a new instance before the previous instance has completed. This is generally not an issue for snapshot creation because it's instantaneous, but may be an issue for snapshot deletion). The main drawback is it's a more difficult to set jobs based on absolute calendar date and time

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

You can have as many backup creation entries in crontab as you want.

**STEP 16: Create a backup retention policy**

This is described [here](https://restic.readthedocs.io/en/latest/060_forget.html#removing-snapshots-according-to-a-policy).

The command you come up with should look like: `AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository forget --ForgetParameters -prune`. Note the lack of reference to the source directories; this command covers *all* the contents of a repository regardless of source.

**STEP 17: Determine the crontab schedule for your backup pruning**

The process for this is exactly the same as it is in Step 14, though it's a good idea to ensure that backups and prunes don't occur simultaneously. Note that while `restic`'s documentation treats backup deletion and pruning as separate operations, for the purposes of this guide they will be treated as being synonymous. 

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

**STEP 19: Put each combined (schedule + `restic` command) command into `/etc/crontab`**

1. Open crontab in Lumina Text Editor via `sudo lte /etc/crontab`. Unlike on (some) Linux distros, crontab can be edited directly in a GUI application on Project Trident.
2. Put each combined command into the crontab file, with each line preceded by a comment describing what the line does. This is helpful for troubleshooting; in a crisis the last thing you want to be doing is trying to divine exactly what each line is doing. See below for what that should look like
3. Save the crontab file and exit Lumina Text Editor

The additional crontab entries should look like this:

```
# Create a backup of source directory daily at 2 PM

`0 14 * * * root AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository backup AbsolutePathToSource`
```

```
# Delete all backups and associated data from repository except for the most recent 7

`@100000 root AbsolutePathToRestic -p $RESTIC_PASSWORD_FILE -r AbsolutePathToRepository forget --keep-last 7 -prune`
