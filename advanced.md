Advanced Installation Topics
============================

The topics discussed in this chapter are intended for users that have a
sound understanding with installing and manipulating open source
software.

If the intent is to install a graphical desktop using the graphical
installer, refer instead to the Install section.

Server Installation
-------------------

The System Selection screen of the |trident| installer can be used to
install a FreeBSD-based command-line server operating system rather than
a graphical desktop operating system. A |trident| server installation
includes the [SysAdm™ API](https://api.sysadm.us/) and [SysAdm™
Client](https://sysadm.us/handbook/client/) for managing the server
locally or remotely.

For a server installation, using the |trident| installer rather than the
FreeBSD installer offers several benefits:

-   The ability to easily configure ZFS during installation.
-   The ability to configure multiple boot environments.
-   A wizard (described in this section) is provided during installation
    to configure the server for first use.

To perform a server installation, start the |trident| installation as
usual. At the System Selection screen of the installer, select
trident Server (console interface only).

Click Next to start the Server Setup Wizard, then click Next again to
see the screen shown in Figure %s \<server2\>.

![Root Password Creation](images/server2b.png)

Input and confirm the root password then click Next to proceed to the
screen shown in Figure %s \<server3\>.

![Primary User Account Creation](images/server3b.png)

For security reasons, do not login as the **root** user. The wizard
requires creating a primary user account for logging into the server.
This account is automatically added to the *wheel* group, allowing the
user to su to the **root** account when administrative access is
required.

Create an account by filling in these fields:

-   **Name:** Can contain capital letters and spaces.
-   **Username:** The name to use for logging in. It cannot contain
    spaces and is case sensitive (e.g. *Kris* is a different username
    than *kris*).
-   **Password:** The password to use for logging in. Type it twice to
    confirm it.
-   **Default shell:** Use the drop-down menu to select the **csh**,
    **tcsh**, **sh**, or **bash** login shell.

When finished, click Next to proceed to Figure %s \<server4\>.

![Hostname Creation](images/server4b.png)

Input the system's hostname. If using ssh to connect to the system,
check Enable remote SSH login. Click Next to proceed to the network
configuration screen shown in Figure %s \<server5\>.

![Network Configuration](images/server5b.png)

Use the Network Interface drop-down menu to choose the desired
interface:

-   **AUTO-DHCP-SLAAC:** (default) Will configure every active interface
    for DHCP and for both IPv4 and IPv6.
-   **AUTO-DHCP:** Will configure every active interface for DHCP and
    for IPv4.
-   **IPv6-SLAAC:** Will configure every active interface for DHCP and
    for IPv6.

Alternately, use the drop-down menu to select the device name for the
interface and manually configure and input the IPv4 and/or IPv6
addressing information. When finished, click Next to access the screen
shown in Figure %s \<server6\>.

![Optional Install Features](images/server6b.png)

To install the FreeBSD ports collection, check Install ports tree then
click Finish to exit the wizard and access the summary screen shown in
Disk Selection.

If installing the server to a system with ZFS already installed, you can
choose to Install to disk or Install into boot Environment.

When installing to disk, click Customize Disk Settings to configure the
system's disk(s). When installing into a Boot Environment, you can
select the ZFS Pool for installation using the drop-down menu.

To save the install configuration for re-use at a later time, insert a
MSDOSFS or FAT32 formatted USB stick and click Save Config to USB.

When ready to continue, click Next. A new window asks if you are ready
to begin the installation. Click Yes to continue or No to continue
modifying the install configuration.

Once the system is installed, it boots to a command-line login prompt.
Login using the primary user account configured during installation. Now
the server can be configured like any other FreeBSD server installation.
The [FreeBSD
Handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/)
is an excellent reference for performing common FreeBSD server tasks.

Restore from Life Preserver backup
----------------------------------

If you have replicated the system's snapshots to a remote backup server,
you can use a |trident| installation media to perform an operating system
restore or to clone another system. Start the installation as usual and
select to Restore from Life Preserver backup in the
System Selection Screen \<install3\>.

Before you can perform a restore, the network interface must be
configured. Click the Network Connectivity (blue circle) icon in order
to determine if the network connection was automatically detected. If
not, refer to the instructions in the Network Manager section of this
handbook and ensure networking is functional before continuing.

Once you are ready, click Restore from Life Preserver backup and Next.
This starts the Restore Wizard. In the **SSH Restore** shown in
Figure %s \<restore2\>, type the IP address of the backup server and the
name of the user account that replicated the snapshots. If the server is
listening on a non-standard SSH port, change the SSH port number.

![: Beginning a SSH Restore](images/restore2a.png)

Click Next and the wizard provides a summary of your selections. If
correct, click Finish. Otherwise, click Back to correct them.

Once the connection to the backup server succeeds, you can select which
host to restore. After making your selection, click Next. The restore
wizard provides a summary of which host it restores from, the name of
the user account associated with the replication, and the hostname of
the target system. Click Finish and the installer proceeds to the
Disk Selection Screen \<install5\>. At this point, you can click
Customize to customize the disk options. However, any ZFS datasets will
be greyed out as they are to be recreated from the backup during the
restore. Once you are finished with any further customizations, click
Next to perform the restore.

Using the System Utilities Menu
-------------------------------

The System Utilities menu is available from the "Emergency Shell" icon
(see insico) in the various |trident| installer screens. Once opened,
you'll see the menu shown in Figure %s \<util1\>.

![System Utilities Menu](images/util1a.png)

This screen provides several options:

-   **shell:** This option is useful when troubleshooting a |trident|
    system that no longer boots. It opens a shell with administrative
    access, including the base FreeBSD utilities. Advanced users can use
    this shell to identify a problem, create a backup or copy essential
    files to another system, or alter configuration files with an editor
    like [ee](https://www.freebsd.org/cgi/man.cgi?query=ee) or vi. When
    finished using the shell, type exit to return to the
    System Utilities Menu \<util1\>.
-   **zimport** This option displays the names of available ZFS pools.
    Type the name of an available pool and the utility imports the pool
    then displays the available boot environments (BEs). Type the name
    of the desired BE and **zimport** mounts the BE then offers to open
    a chroot shell so the environment's contents can be viewed and
    edited as needed in order to perform maintenance on the boot
    environment. When finished, type exit to leave the boot environment
    and return to the System Utilities Menu \<util1\>.
-   **fixgrub:** This option is scheduled for removal as GRUB is no
    longer supported by |trident|.
-   **exit:** This option returns the user to the main
    trident Installation Menu \<install1\>.

Automated Installations
-----------------------

|trident| provides a set of Bourne shell scripts to allow advanced users
to create automatic or customized |trident| installations. pc-sysinstall
is the name of the master script. The script reads a customizable
configuration file and uses dozens of backend scripts to perform the
installation. Read more about this utility by typing man pc-sysinstall.

Here is a quick overview of the components used by pc-sysinstall:

-   /usr/local/share/pc-sysinstall/backend/ contains the scripts used by
    the |trident| installer. Scripts have been divided by function, such
    as functions-bsdlabel.sh and functions-installcomponents.sh. To
    learn more about how the |trident| installer works, read through
    these scripts. This directory also contains the parseconfig.sh and
    startautoinstall.sh scripts which pc-sysinstall uses to parse the
    configuration file and begin the installation.
-   /usr/local/share/pc-sysinstall/backend-query/ contains the scripts
    used by the installer to detect and configure hardware.
-   /usr/local/share/pc-sysinstall/conf/ contains the configuration file
    pc-sysinstall.conf. It also contains a file indicating which
    localizations are available (avail-langs), an exclude-from-upgrade
    file, and a licenses/ subdirectory containing text files of
    applicable licenses.
-   /usr/local/share/pc-sysinstall/doc/ contains the help text seen if
    pc-sysinstall is run without any arguments.
-   /usr/local/share/pc-sysinstall/examples/ contains several example
    configuration files for different scenarios (e.g. upgrade and
    fbsd-netinstall). The README in this directory should be considered
    as **mandatory** reading before using pc-sysinstall.
-   /usr/sbin/pc-sysinstall is the script used to perform a customized
    installation.

This section discusses the steps needed to create a custom installation.

First, determine which variables to customize. A list of possible
variables can be found in /usr/local/share/pc-sysinstall/examples/README
and are summarized in Table %s \<insvars\>.

> **note**
>
> This table is meant as a quick reference to determine which variables
> are available. The README in /usr/local/share/pc-sysinstall/examples/
> contains more complete descriptions for each variable.

Next, create a customized configuration. One way to create a customized
configuration file is to read through the configuration examples in
/usr/local/share/pc-sysinstall/examples/ and follow the most relevant
example. Copy the file to any location and customize it so it includes
the desired variables and values in the installation.

An alternate way to create this file is to start an installation,
configure the system as desired, and save the configuration to a USB
stick (with or without actually performing the installation). Use the
saved configuration file as-is, or customize it to meet an
installation's needs. This method may prove easier when performing
complex disk layouts.

To perform a fully automated installation which does not prompt for any
user input, review
/usr/local/share/pc-sysinstall/examples/pc-autoinstall.conf and place a
customized copy of the file into /boot/pc-autoinstall.conf on the
installation media.

Table %s \<autovars\> summarizes the additional variables available for
fully automatic installations. More detailed descriptions can be found
in the /usr/local/share/pc-sysinstall/examples/pc-autoinstall.conf file.

> **note**
>
> The variables in this file use a different syntax than those in
> Customizing a trident Installation \<insvars\> as the values follow a
> colon (:) and a space rather than an = sign.

Finally, create a custom installation media or installation server.
pc-sysinstall supports two installation methods:

1.  From CD, DVD, or USB media.
2.  From an installation directory on an HTTP, FTP, or SSH+rsync server.

The easiest way to create a custom installation media is to modify an
existing installation image. For example, if an ISO for the |Trident|
version to customize is downloaded, the superuser can access the
contents of the ISO with a few commands:

``` {.sourceCode .none}
[name@example] mdconfig -a -t vnode -f Trident-Desktop-2018-08-01-x64-DVD.iso.md5 -u 1

[name@example] mount -t cd9660 /dev/md1 /mnt
```

Make sure to cd into the desired destination directory for the copied
ISO contents. In the next examples, /tmp/custominstall/ was created for
this purpose:

``` {.sourceCode .none}
[name@example] cd /tmp/custominstall

[name@example] tar -C /mnt -cf - . | tar -xvf -

[name@example] umount /mnt
```

Alternately, if an installation CD or DVD is inserted, mount the media
and copy its contents to the desired directory

> ``` {.sourceCode .none}
> ```
>
> [<name@example>] mount -t cd9660 /dev/cd0 /mnt
>
> [<name@example>] cp -R /mnt/\* /tmp/custominstall/
>
> [<name@example>] umount /mnt

If creating an automated installation, copy the customized
pc-autoinstall.conf to /tmp/custominstall/boot/.

Copy the customized configuration file to /tmp/custominstall/.
Double-check the installMedium= variable in the customized configuration
file is set to the correct installation media.

Adding extra files may be necessary if certain variables are set in the
custom configuration file:

-   **installComponents=** Any extra components to install must exist in
    extras/components/.
-   **runCommand=** The command must exist in the specified path.
-   **runScript=** Make sure the script exists in the specified path.
-   **runExtCommand=** Ensure the command exists in the specified path.

If the installation media is a CD or DVD, create a bootable media
containing the files in the directory. To create a bootable ISO:

``` {.sourceCode .none}
[name@example] cd /tmp/custominstall

[name@example] mkisofs -V mycustominstall -J -R -b boot/cdboot -no-emul-boot -o myinstall.iso
```

Use a preferred burning utility to burn the ISO to the media.

To begin an installation that requires user interaction, type
pc-sysinstall -c /path\_to\_your\_config\_file

To begin a fully automated installation, insert the installation media
and reboot.

If using an HTTP, FTP, or SSH server as the installation media, untar or
copy the required files to a directory on the server accessible to
users. Be sure to configure the server so installation files are
accessible to the systems to install.
