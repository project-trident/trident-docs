Author: [jdrch](https://github.com/jdrch)

Last updated: 2019-07-17

**How to set up regular recurring, recursive, incremental, online filesystem backups using `zfsnap`**

*What this guide will get you:*

* The creation of snapshots from which you can restore all practical (there are some that do not make sense to back up from a UNIX perspective) filesystems from historical points in time with equal time gaps between them
* Automatic snapshot pruning (deletion when they reach a certain age, set by the user)
* Snapshots that contain only changes since the previous snapshot
* Snapshots happen instantaneously and while the computer and filesytems are both online and mounted
* Snapshots on the same physical block device as the source data

*What this guide will NOT get you:*

* Device (entire PC, including bootloader, etc.) backup. For that, try [Bareos](https://www.bareos.org/en/), [Bacula](https://www.bacula.org/), or [Amanda](http://www.amanda.org/). No claim about those tools is made or implied by this statement
* Block/physical storage device redundancy. If the physical storage device(s) your pool is on stops functioning, your original data and snapshots will be lost. To avoid this, use [`zfs send`](https://www.freebsd.org/cgi/man.cgi?zfs(8)) or some similar solution to send snapshots to another ZFS pool

**STEP 0: Get the right mindset**

Be prepared to NOT understand things at first, but also be patient. Eventually you'll get the hang of what you're doing. 

Also, because snapshots are just backups, and `zfsnap` operates on those exclusively, there is minimal risk to your data during setup. The mistake you're most likely to make is setting `zfsnap` to run too often, which might slow down the machine until the problem is discovered and rectified.

Advanced users may find this guide a bit like spoonfeeding, but it's deliberately written in this manner to enable people unfamiliar basic programming concepts and who do better with GUIs than CLIs to understand how crontab and the commands it calls work. Too many Linux/Unix guides assume a lot of prior knowledge for which no easily accessible corresponding guide or documentation exists, leaving users struggling to understand what they need to do to get their desired effects.

There are many links and citations to allow users to do as much (or as little) background reading as they want or need.

**STEP 1: Read the `zfsnap` documentation**

Read:

1. The [introductory writeup](https://www.zfsnap.org/docs.html) at the project website
2. The [`zfsnap` man page](https://www.zfsnap.org/zfsnap_manpage.html)

As with most Linux/Unix documentation, 1.2 may be pretty dense reading, but it's the official, canonical description of the functionality this guide uses, so patiently go through it.

**STEP 2: Read FreeBSD's crontab man page**

While `zfsnap` creates, names, and automatically prunes backups, it uses `cron` to invoke it automatically. As such, you'll have to be familiar with crontab syntax to create the zfsnap run schedule you want. crontab syntax varies based on OS implementation, and Project Trident (ultimately) [uses FreeBSD's implementation thereof](https://t.me/ProjectTrident/33871). 

Read:

1. The [crontab man page](https://www.freebsd.org/cgi/man.cgi?crontab(5))
2. [Configuring `cron`](https://www.freebsd.org/doc/handbook/configtuning-cron.html) in the FreeBSD Handbook

The same dense reading warning applies. 

**STEP 3: Download and install `zfsnap`**

1. On the Project Trident desktop, open the Lumina start menu
2. Search for *AppCafe*
3. In the window that opens, search for `zfsnap`. You may see multiple versions listed; you can read about their differences [here](https://github.com/zfsnap/zfsnap). This guide will use the latest version available in Project Trident (2.x at this writing)
4. "Install" (this term is used loosely as `zfsnap` is a script that makes use of built-in ZFS functions/programs, not an application itself) the latest `zfsnap` version

**STEP 4: Determine the backups you want**

Although `zfsnap` doesn't use this terminology, snapshots can be divded into "families" based on everything in a given `zfsnap snapshot` command *after* the `zfsnap snapshot` part. The significance of this is each family requires only **ONE** crontab entry for creation. Put another way, a *single* crontab entry can generate *multiple* snapshots of the same type (family).

Also important to keep in mind is that - within the scope of this discussion - `zfsnap snapshot` covers only which filesystems will have snapshots of them made and their minimum retention time. *When* and *how often* said snapshots are created is covered via the rest of the corresponding crontab entry, as will be shown later.

1. Determine the [zpools](https://wiki.ubuntu.com/ZFS/ZPool) (ZFS storage pools, yes that's an Ubuntu documentation link, but the definition of zpool is universal across ZFS implementations) in your system by using [`zpool list`](https://docs.oracle.com/cd/E19253-01/819-5461/gamml/index.html)
2. Select the zpool(s) whose filesytems you want to back up using the [`zfsnap snapshot`](https://www.zfsnap.org/zfsnap_manpage.html#snapshot) command
3. Build the required `zfsnap snapshot` command accordingly 

An example `zfsnap snapshot` command based on the above is: `zfsnap snapshot -rv -a 6w zpool`. All snapshots created from this command are in the same "family," and so require only 1 crontab line. The aforesaid command translates to:

* Invoke `zfsnap` (represented by by the absolute path to the `zfsnap` command, `/sbin/zfsnap`) to ...
* Create snapshots (represented by `snapshot`) ...
* Individually, (also known as "recursively," represented by `-rv`) with ...
* A minimum retention period (represented by `-a`, also called [TTL](https://en.wikipedia.org/wiki/Time_to_live) but "minimum retention period" is easier to understand as will be shown later) of ...
* 6 weeks (represented by `6w`) of ...
* All filesystems on the zpool named `zpool` (represented by `zpool`)

Or, put into one sentence: Create recursive snapshots of all filesystems on zpool named `zpool` with a minimum retention period of (read "that should be deleted after") 6 weeks. It may be easier to understand the syntax if you read the translation first before reading the command.

Test each of your `zfsnap snapshot` commands in QTerminal using the use the `-n` (dry-run) and `-v` (verbose) flags to make sure the command does what you think it does, e.g. `zfsnap snapshot -n -v -rv -a 6w zpool`.

**STEP 5: Determine the crontab schedule for your backups**

1. Think about how often you want to create backups
2. The above bulletpoint is determined entirely by crontab, so *write down* a crontab schedule that matches the above, based on the crontab syntax in 2.1 above

A couple details about 5.2 above:

* crontab fields support whole numbers only, e.g. `0 2 * * *` will work, `0 2.2 * * *` will not work
* /*n*, where *n* is a whole number, e.g. `0/5`, does not work (the way you might think) for the Minutes field. Without going into details, just avoid it
* The `@`*n*`s` syntax, where *n* is a whole number, e.g. `@1000s`, is much easier to understand than the individual fields. In addition, it ensures that each successive invocation happens *n* seconds *after the previous one has completed*, which means tasks in the same family (crontab entry) never collide (read: attempt to start a new instance before the previous instance has completed. This is generally not an issue for snapshot creation because it's instantaneous, but may be an issue for snapshot deletion). The main drawback is it's a more difficult to set jobs based on absolute calendar date and time

An example of a crontab schedule is `0	14	*	*	*` (tab separated), which translates to:

* Every time the system clock time value is 0 minutes, 14 hours (represented by `0 14`, evaluates to 14:00/2:00 PM) ...
* Regardless of the day of the month, the month, or the day of the week (what `* * *` stand for, respectively)

Or, put into one sentence: Every day at 14:00/2 PM.

**STEP 6: Match desired backups with their corresponding crontab schedules to create single, complete crontab entries for each backup**

For example, putting the examples in Steps 5 and 4 together - in that sequence - into a sample crontab entry gives:

`0 14 * * * /sbin/zfsnap snapshot -rv -a 6w zpool`

Which translates to: 

* Every time the system clock time value is 0 minutes, 14 hours (represented by `0	14`, evaluates to 14:00/2:00 PM) ...
* Regardless of the day of the month, the month, or the day of the week (what `* * *` stand for, respectively) ...
* Invoke zfsnap (represented by by the absolute path to the `zfsnap` command, `/sbin/zfsnap`) to ...
* Create snapshots (represented by `snapshot`) ...
* Individually, (also known as "recursively," represented by `-rv`) with ...
* A minimum retention period (represented by `-a`, also called [TTL](https://en.wikipedia.org/wiki/Time_to_live) but "minimum retention period" is easier to understand as will be shown later) of ...
* 6 weeks (represented by `6w`) of ...
* All filesystems on the zpool named `zpool` (represented by `zpool`)
 
Or, put into one sentence: Create a recursive snapshot of all filesystems on zpool named `zpool` every day at 14:00/2 PM with a minimum retention period of (read "that should be deleted after") 6 weeks.

You can have as many snapshot creation entries (snapshot families) in crontab as you want.

Clearly, from the above, a snapshot will be created each day at 14:00/2 PM. After 10 days, for example, there will be 10 snapshots created from that single line. This is what is meant by the snapshot "families" concept introduced in Step 4.

**STEP 7: Decide when you want to delete (`zfsnap destroy`/prune) snapshots whose age is greater than their specified minimum retention time (read: old snapshots)**

There are a lot of options for this, but to keep things simple this guide will cover `destroy`ing *all* old snapshots at once. 

A snapshot's minimum retention time (TTL) and the cadence of `zfsnap destroy` are related *only* in the sense that the latter will delete snapshots older than their minimum retention time *when* a `zfsnap destroy` matching said snapshot (to be covered later) is invoked*. In other words, a snapshot will surive beyond its minimum retention time until the next `zfsnap destroy` invocation that matches it. The `zfsnap destroy` command provided in this guide will match *all* snapshots older than their minimum retention time.

As an example, consider a snapshot with a minium retention time of 2 hours, taken at midnight (00:00). A matching `zfsnap destroy` invocation at 1 hour after that snapshot was taken, at 01:00 on the same day, will leave that snapshot intact. However, a matching  `zfsnap destroy` invocation at 3 hours after that snapshot was taken, at 03:00 on the same day, will delete that snapshot. 

Of note is the fact that the snapshot was retained for 3 hours despite its minimum retention time being 2 hours. That is why, to this point, this guide uses the term "minimum retention time" instead of "TTL": it better and more plainly describes the meaning of that parameter. "TTL" implies that whatever it refers to "dies" when the TTL value is reached, while "minimum retention time" conveys that whatever it refers to will live for *at least* the minimum retention time. Henceforth, the guide will use the official term, TTL, to align with the documentation.

The steps, therefore, are:

1. Decide how often/when old snapshots should be deleted. As the documentation states, while snapshot creation is (mostly) instantaneous, deletion takes longer and can be taxing to the machine. This is exacerbated by the fact the guide covers deleting all old snapshots at once, for the sake of simplicity. As such, it is highly recommended that `zfsnap destroy` be scheduled with sufficient time between invocations, that it be regular but not be *too* frequent, AND that the `@`*n*`s` syntax is used to prevent task collision
2. Build the crontab schedule using the same syntax as Step 5
3. Add `zfsnap destroy -rv zpool` to 7.2 above

An example of a combined command for the above is:

`@100000s /sbin/zfsnap destroy -rv zpool`

Which translates to: 

* 100000s after the completion of the previous invocation of the following command by `cron` (represented by `@100000s`) ...
* Invoke zfsnap (represented by by the absolute path to the `zfsnap` command, `/sbin/zfsnap`) to ...
* Destroy (represented by `destroy`) ... 
* Individual snapshots (also known as recursive snapshots, represented by `-rv`) whose age is greater than their TTL value on ...
* All filesystems on the zpool named `zpool` (represented by `zpool`)

Or, put into a single sentence: Destroy all old snapshots in all filesystems on zpool 100000 seconds after the last such destruction completed.

Test each of your `zfsnap destroy` commands in QTerminal using the use the `-n` (dry-run) and `-v` (verbose) flags to make sure the command does what you think it does, e.g. `zfsnap destroy -n -v -rv zpool`.

You can have as many deletions as you want, especially if you prefer to prune (delete members of) only certain snapshot families at a time. The full syntax for `zfsnap destroy`, which enables that, is [here](https://www.zfsnap.org/zfsnap_manpage.html#destroy). Both it and corresponding examples are omitted here for the sake of simplicity.

**STEP 8: Put each combined (schedule + `zfsnap` command) command into `/etc/crontab`**

1. Open crontab in Lumina Text Editor via `sudo lte /etc/crontab`. Unlike on (some) Linux distros, crontab can be edited directly in a GUI application on Project Trident.
2. Put each combined `zfsnap snapshot` and `zfsnap destroy` command into the crontab file, with each line preceded by a comment describing what the line does. This is helpful for troubleshooting; in a crisis the last thing you want to be doing is trying to divine exactly what each line is doing. See below for what that should look like
3. Save the crontab file and exit Lumina Text Editor

The additional crontab entries should look like this:

```
# Create a recursive snapshot of zpool daily at 2 PM with a TTL of 6 weeks

0 14 * * * /sbin/zfsnap snapshot -rv -a 6w zpool
```

```
# Destroy all old snapshots on zpool 100000 seconds after last destruction completed

@100000s /sbin/zfsnap destroy -rv zpool
```

**(Later on) STEP 9: Verify that your snapshots are being created as you want**

1. Depending on the schedule you set, wait some time for enough snapshots to have been taken and deleted
2. List all snapshots using [`zfs list -t snapshot`](https://docs.oracle.com/cd/E19253-01/819-5461/gbiqe/index.html). The output should match your crontab entries

If you made a syntax error in crontab resulting in too many snapshots being taken:

1. Correct the problematic crontab entry first as shown in Step 8. This will stop the excessive snapshot creation 
2. In QTerminal, run a `zfsnap destroy -rv zpool` (replace `zpool` with the name of the pool in question) on the affected pools. This will delete all snapshots on the referenced pools created by `zfsnap`, so that your crontab entries can start creating snapshots afresh
