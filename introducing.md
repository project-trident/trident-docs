# Introduction

Welcome to Project Trident!

Project Trident is a desktop distribution of [TrueOS](https://www.trueos.org), which is a rolling-release variant of [FreeBSD](https://www.freebsd.org).
The goal of the Project is to provide a simple, secure, and highly usable FreeBSD experience.
Project Trident is completely independent and financially backed through the generous contributions of the Open Source community.

[Project Trident](https://www.Trident.org) began in 2018 when TrueOS was reorganized into a scaled down server installation of FreeBSD.
The discontinued desktop portion of TrueOS was taken by Ken Moore and J.T. Pennington and rebuilt into Project Trident.

Project Trident is essentially a customized installation of TrueOS and FreeBSD, not a forked derivative.
Project Trident has a simple graphical installer that has partition support and other customization.
The underlying TrueOS/FreeBSD system is kept intact and provides a fully functional FreeBSD system.
Other differences from FreeBSD include:

-   Project Trident pre-configures the BSD-licensed Lumina desktop environment.
    Additional desktop environments can be installed and appear in the graphical login menu.

-   The Project Trident installer supports configuring ZFS partitions during installation.

-   Project Trident has both graphical and command line software management systems.

-   Project Trident provides many graphical utilities for system configuration and management.

-   Project Trident comes pre-configured with a number of automatic scripts connecting USB memory sticks.

-   The Project Trident boot menu supports boot environments or snapshots of the operating system.
    The System Update Manager automatically creates a new boot environment before every update.
    If an update fails, the system can be rebooted into an earlier boot environment.
    This allows for easy recovery if any issues happen during the update process.

These articles cover the installation and use of Project Trident.
All documentation must be considered a "work in progress" and is wholly dependent on Project Trident community contributions.

## Features

Project Trident provides many features:

-   **Easy installation:** Insert the installation media and reboot the system to start the installer.
    Fill in the prompts in the installation menus.

-   **Automatically configured hardware:** Video, sound, network, and other devices automatically configure during installation.

-   **Customizable desktop interface:** Project Trident installs the Lumina desktop by default.
    Additional desktop environments can also be installed to support user preferences.

-   **Easy software management:** The AppCafe makes installing, upgrading, and uninstalling software safe and easy.

-   **Lots of software available:** Most software ported to FreeBSD is available on Project Trident.
    There are currently over 26,100 applications available on FreeBSD.

-   **Easy to update:** Project Trident has a built-in System Update Manager.
    This notifies the user about available updates and makes it easy to apply TrueOS security fixes, bug fixes, and system enhancements.
    Additionally, the Update Manager is used to upgrade the operating system or update installed software.

-   **No fragmentation:** Project Trident hard drives never need defragmenting and are formatted with OpenZFS, a self-healing filesystem.

-   **Laptop support:** Provides power saving and automatic switching between wired and wifi network connections.
    The rolling release model of Project Trident provides an environment to quickly add support for new hardware.

-   **Easy system administration:** Project Trident provides many graphical tools for performing system administration.

-   **Vibrant community:** Project Trident has a friendly and helpful community.

### Security

The Project Trident system is secure by default.
This section is an overview of the built-in security features.
There are also tips about increasing the security of the installed system beyond the configured defaults.

The security features built into Project Trident include:

-   **Naturally immune to viruses and other malware:** Most viruses are written to exploit the Windows operating system.
    These are incompatible with the binaries and paths found on a Project Trident system.
    Additional antivirus software is also available in the Appcafe.
    This is useful when sending or forwarding email attachments to users running other operating systems.

-   **Potential for serious damage is limited:** Privilege separation between users and the administrator account (root) are built-in.
    Files and directories can only be modified by root any any users and groups with permission.
    Any executed programs or scripts are only granted the permissions of that user.
    A malicious program can only infect the files and directories owned by the user.
    Core operating system files are protected.
    Only users that are *wheel* and/or *operator* group members can gain administrative access.
    These users are still not allowed to list directory contents or access files outside of the set "user" and "group" permissions.

-   **Built-in firewall:** The default firewall ruleset allows Internet access and any available network shares.
    The firewall does not allow any inbound connections to the computer.

-   **Few default services:** All boot services can be viewed in the Service Manager.
    Service Manager also allows starting, stopping, and adding or removing from boot any system service.

-   **SSH is disabled:** SSH can only be enabled by the administrator (root).
    This prevents bots and outside individuals from accessing the Project Trident system.
    If SSH access is required, add `sshd\_enable=YES` to **/etc/rc.conf**.
    Then, start the service with the Service Manager or by typing `sudo service sshd start` in the command line.
    Root access is required.
    A firewall rule must also be added using the Firewall Manager.
    Allow SSH connections through the default SSH TCP port *22*.

-   **SSH root logins are disabled:** If SSH is enabled, login as a regular user and use `su` or `sudo` for administrative actions.
    Do not change this setting, as it prevents an unwanted user from having complete access to the system.

-   **sudo is installed:** `sudo` allows users in the *wheel* group permission to run an administrative command after typing the user password, not the *root* password.
    The first user created during installation is added to the *wheel* group.
    Use the User Configuration in Desktop Settings to add other users to the *wheel* group.
    To change the default `sudo` configuration, use `visudo` as *root*.
    This command verifies there are no syntax errors, which could inadvertently prevent root access.
    
-   [AES instruction set](https://en.wikipedia.org/wiki/AES_instruction_set) (AESNI) support is loaded by default for the Intel Core i5/i7 processors that support this encryption set.
    This support speeds up AES encryption and decryption.
    
-   **Automatic notification of security advisories:** The System Update Manager utility automatically checks for any updates available from a [security advisory](https://www.freebsd.org/security/advisories.html) that affects Project Trident.
The administrator can keep the operating system fully patched against vulnerabilities with a mouse click.

-   Tor Mode can be used to anonymously access Internet sites as it automatically forwards all Internet traffic through the [Tor Project's](https://www.torproject.org/) transparent proxy service.

To learn more about security on TrueOS and Project Trident systems, `man security` is a good place to start.
These resources provide more information about security on FreeBSD-based operating systems:

-   [FreeBSD Security Information](https://www.freebsd.org/security/)
-   [Security Section in the FreeBSD Handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/security.html)
-   [Hardening FreeBSD](http://www.bsdguides.org/2005/hardening-freebsd/)

### ZFS Overview

ZFS is an enterprise grade file-system with many features.
Support for high storage capacities, high reliability, the ability to quickly take snapshots, boot environments, continuous integrity checking and automatic repair, RAIDZ designed to overcome hardware RAID limitations, and native NFSv4 ACLs are all ZFS features seen in Project Trident.

The Wikipedia entry on ZFS provides an excellent starting point to learn about its features.
[FreeBSD Mastery: ZFS](https://www.michaelwlucas.com/os/fmzfs) by Michael W Lucas and Allan Jude is also a helpful ZFS and FreeBSD resource.

These resources are also useful:

-   [ZFS Evil Tuning Guide](https://www.solaris-cookbook.eu/solaris/solaris-10-zfs-evil-tuning-guide/)
-   [FreeBSD ZFS Tuning Guide](https://wiki.FreeBSD.org/ZFSTuningGuide)
-   [ZFS Best Practices Guide](https://documents.irf.se/get_document.php?group=Computer&docid=311)
-   [ZFS Administration Guide](https://docs.oracle.com/cd/E19253-01/819-5461/index.html)
-   [Becoming a ZFS Ninja (video)](https://blogs.oracle.com/video/becoming-a-zfs-ninja)
-   [Blog post explaining ZFS storage stack simplification](https://blogs.oracle.com/bonwick/rampant-layering-violation)

Project Trident Comparisons
--------------------

As Project Trident grows and evolves, many users appreciate comparisons with
other operating systems. These comparisons are intended to help new
users understand the abilities and features available when deciding to
install Project Trident. Accuracy is a chief concern.

### FreeBSD and PC-BSD

These features or enhancements were introduced with Project Trident and now
separate Project Trident from |pcbsd|:

> **note**
>
> |pcbsd| and FreeBSD are placed together as both are very similar
> "under the hood". The differences for either OS to Project Trident are listed
> here.

-   Based on FreeBSD-CURRENT.
-   The GRUB bootloader has been replaced by the FreeBSD bootloader,
    which now provides both GELI and boot environment support.
-   **Quick boot times with OpenRC:** Project Trident is using
    [OpenRC](https://github.com/OpenRC/openrc) as part of the init
    process which allows services to be started in parallel. This
    results in dramatically improved system boot times for Project Trident.
    OpenRC also improves general service management. One example is the
    ability to automatically run when new elements are introduced to the
    system, such as plugging in an Ethernet cable. Using OpenRC allows
    Project Trident to use some system services that are different from
    FreeBSD. These differences are listed in Table %s \<sysserv\>

    > **note**
    >
    > The sysserv table is updated as development continues on the
    > Project Trident implementation of OpenRC. For a complete list of all
    > available services in OpenRC, see rcuprnlvl.

-   A Project Trident installation includes the |lumina| Desktop. Additional
    window managers and desktop environments can be installed using the
    |appcafe|. Meta packages are available for popular desktop
    environments to allow easy installation of all required packages.
-   The [SysAdm™ Client](https://sysadm.us/handbook/client/) and
    [Server](https://sysadm.us/handbook/server/) has replaced Control
    Panel. Most of the utilities from Control Panel are rewritten to use
    the |sysadm| middleware. Under the hood, |sysadm| provides REST and
    WebSocket APIs for securely managing local or remote FreeBSD and
    Project Trident systems.
-   Many utilities have been converted to the |sysadm| API and many more
    are available through [SysAdm](https://sysadm.us/handbook/client/):
    -   AppCafe
    -   Update Manager
    -   Boot Environments
    -   Devices
    -   Firewall
    -   Mouse Settings
    -   Services
    -   System Controls
    -   Tasks
    -   Users
    -   Life Preserver
-   The functionality provided by the *About* utility is incorporated
    into Lumina Information \<luminautl.html\#information\>.
-   The functionality provided by the
    Service Manager \<service-manager\> (pc-servicemanager) is
    integrated into |sysadm|.
-   The Active Directory & LDAP utility (pc-adsldap) is deprecated.
-   Login Manager (pc-dmconf) is replaced by pcdm-config).
-   System Manager (pc-sysmanager) is deprecated.
-   freebsd-update is retired in favor of using pkg for system updates.
-   The option to use the scfb display driver is added to the installer.
    This driver is suitable for newer UEFI laptops as it automatically
    detects the native resolution. This is a good solution for newer
    Intel hardware that would otherwise require drivers that have not
    been ported to FreeBSD. Before selecting this driver, check the BIOS
    and ensure the CSM module is disabled.

> **note**
>
> Depending on the system hardware, the scfb driver may not
> :   support a dual-head configuration, for example, using an external
>     port for presentations. Some hardware will support multi-monitors
>     using the scfb driver but is dependant on how the graphics are
>     embedded onto the hardware and which ports are attached to which
>     video card(s). Support for suspend and resume is also dependant on
>     manufacture implemenatation. See man 4 scfb and man 4 acpi for
>     additional information.
>
-   Customize is removed from the System Selection screen in order to
    reduce the size of the installation media. Additional software can
    be installed post-installation using |appcafe|.
-   The Boot to console (Disable X) option has been added to the
    graphical boot menu.
-   The graphical and command line versions of PBI Manager and Warden
    are removed.
-   pc-thinclient is removed as it is deprecated.

### Linux and Project Trident

Project Trident is based on FreeBSD, meaning it is not a Linux distribution.
While there are many similarities with Linux, some features have
different names and some commands have different flags or output on a
BSD based system. This section will cover some of these differences.

BSD and Linux use different filesystems. Many Linux distros use EXT2,
EXT3, EXT4, or BTRFS, while Project Trident uses UFS or OpenZFS. In order to
dual-boot with Linux or access data on an external drive formatted with
another filesystem, it is imperative to research if the filesystem used
is accessible to both operating systems.

Table %s \<filesys support\> summarizes the various filesystems commonly
used by desktop systems. Project Trident automatically mounts several
filesystems: *FAT16*, *FAT32*, *EXT2*, *EXT3* (without journaling),
*EXT4* (read-only), *NTFS5*, *NTFS6*, and *XFS*.

> > **note**
> >
> > A comparison of some popular graphical file management
> > :   utilities available in Project Trident can be found in the
> >     Files and File Sharing section.
> >
> **note**
>
> exFAT partitions can be mounted read/write on FreeBSD using the
> *fusefs-exfat* package. Due to the Microsoft license used for exFAT,
> the package cannot come pre-installed by the OS. The user must
> manually install the *fusefs-exfat* package using |appcafe| or
> pkg install fusefs-exfat on the command line. When complete, the
> Project Trident automount systems are already aware of exFAT and are able to
> automatically mount/access the devices as needed.

Linux and BSD use different naming conventions for devices. Here are
some examples:

-   Linux Ethernet interfaces begin with eth, while BSD interface names
    indicate the name of the driver used to make the device function. An
    Ethernet interface named re0 indicates it uses the Realtek re
    driver. One advantage of this convention is the easy ability to find
    the respective man page. For the re driver issuing man 4 re will
    open the man page for the re driver which will list which models and
    features are provided by the driver. This convention applies to all
    drivers. man 4 wlan will open the wlan man page containing all wlan
    driver information.
-   BSD disk names differ from Linux. IDE drives begin with ada and SCSI
    and USB drives begin with da. Following the convention of
    informative device names, BSD applies this to disk drives as well.
    da0p1 is the 1st partition on the 1st USB/SCSI drive. da0p2 is the
    2nd partition on the 1st USB/SCSI drive.

> **tip**
>
> This convention continues with subsequent drives. da1p3 would be the
> 3rd partition on the 2nd USB/SCSI drive and ada4p6 would be the 6th
> partition on the 5th IDE drive. Physical drive numbering begins at 0,
> while the partition numbers on the drive start at 1.

Some of the features used by BSD have similar counterparts to Linux but
the name of the feature may differ. Table %s \<feature names\> provides
some common examples:

Users comfortable with the command line may find some of the common
Linux commands have different names on BSD. Table %s \<common commands\>
lists some common BSD commands and what they are used for.

There are many articles and videos which provide additional information
about some of the differences between BSD and Linux:

-   [Comparing BSD and
    Linux](https://www.freebsd.org/doc/en/articles/explaining-bsd/comparing-bsd-and-linux.html)
-   [FreeBSD Quickstart Guide for Linux
    Users](https://www.freebsd.org/doc/en/articles/linux-users/index.html)
-   [BSD vs
    Linux](http://www.over-yonder.net/~fullermd/rants/bsd4linux/01)
-   [Why Choose
    FreeBSD?](https://www.freebsd.org/advocacy/whyusefreebsd.html)
-   [Interview: BSD for Human
    Beings](https://www.unixmen.com/bsd-for-human-beings-interview/)
-   [Video: BSD 4 Linux
    Users](https://www.youtube.com/watch?v=xk6ouxX51NI)
-   [Why you should use a BSD style license for your Open Source
    Project](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)
-   [A Sysadmin's Unixersal Translator (ROSETTA
    STONE)](http://bhami.com/rosetta.html)

### Trident and Windows

Project Trident uses several similar, but different elements to their
counterparts on Windows. Table %s \<troswinapps\> highlights a few of
these:

> **note**
>
> This table isn't meant to be an exhaustive listing of applications but
> simply provides a few Trident/FreeBSD equivalents for users familiar
> with their previous operating system.

Here are a few resources that go into greater detail examining the
differences between Windows and BSD:

-   [FreeBSD is NOT Windows](http://vtbsd.net/notwindows.html)
-   General [Comparison of Operating
    Systems](https://en.wikipedia.org/wiki/Comparison_of_operating_systems)
-   [Open Source Alternatives](https://opensource.com/alternatives)

### Mac OS X and Trident

Mac OS X is related to FreeBSD, resulting in some system level
similarities. Many of the features that make Mac OS X a popular
operating system are found in Project Trident. Project Trident, like Mac OS X,
includes built in securtiy features and access to many free programs and
apps. Mac OS X comes with some of these programs pre-installed, but
Project Trident users can use the App Cafe to download programs that fill many
of the same functions. Project Tridentis intentionally a light weight install.
This allows for easy user customization.

### Virtualization

A virtualized environment allows users to test drive an operating system without overwriting the current operating system.
This is an excellent way to test drive an operating system, determine hardware support, or try multiple operating system versions.
Virtualization software creates a Virtual Machine (VM), a software-created computer environment that can install and run an operating system.
The only limitation to virtualization is the system hardware.
The virtual environment operating system can run slowly if the host computer has limited amounts CPU and RAM.
Closing other non-essential applications on the host computer can free more CPU and RAM for the virtual machine.

#### bhyve

bhyve (pronounced bee hive) is a type-2 hypervisor that runs natively on Project Trident.
bhyve can run FreeBSD 9+, OpenBSD, NetBSD, Linux, and Windows guests.
Current development efforts aim at widening support for other operating systems for the x86-64 architecture.
The [FreeBSD Handbook Virtualization](https://www.freebsd.org/doc/handbook/virtualization-host-bhyve.html) section has in-depth instructions about bhyve features and use.
bhyve is still under active development and may not have a complete user experience yet.

Many users prefer VirtualBox for a more user-friendly virtualization experience/

#### VirtualBox

VirtualBox is a popular virtualization software available for Project Trident.
Installing VirtualBox through the AppCafe or typing `sudo pkg install virtualbox-ose` on the command line installs all required dependencies.
Installing the *virtualbox-ose-additions* package (also known as VirtualBox Guest Additions) can greatly improve the performance of Project Trident or other guest operating systems.
The guest additions add mouse pointer integration, shared folders between the host and guest (depending on the guest OS), improved video support, and a shared clipboard.

VirtualBox does not currently support the shared folders feature with a Project Trident guest.
To share files between the host and a Project Trident guest, use an NFS share.

Please see the [VirtualBox website](https://www.virtualbox.org/) for additional information.
The [VirtualBox Guest Additions](http://www.virtualbox.org/manual/ch04.html) page has support information and usage instructions.

Running VirtualBox on a Project Trident system for the first time automatically gives the user account that started VirtualBox the necessary permissions to run the application.
This could break existing shortcuts to VirtualBox.
To fix a broken shortcut, log out and in again.

#### Creating a Virtual Machine for a Project Trident Install

This section describes how to prepare VirtualBox for an installation of Project Trident using an *.iso* file.

Install VirtualBox on the host system, download a Project Trident ISO from [the website](https://www.Trident.org/downloads/),  and create a new virtual machine to install Project Trident as a guest OS.
Configure the virtual machine (VM) for a Project Trident guest:

- Assign memory to the VM: 4 GB is recommended, but the system can run slowly with 2 GB.
- Create a 30-50 GB virtual disk for the guest operating system and any downloaded software.
  Adjust the virtual disk size as necessary to match the intended use for the operating system, but the virtual disk should not be less than 15 GB.
- Open the settings for the VM after it is created and set the network adapter to be *bridged*.
  This will allow the guest operating system to connect to the Internet through the host system.

Start VirtualBox to begin creating the VM.

![VirtualBox Menu](images/vbox1a.png)

Click *New* to start the new virtual machine wizard.

![Create Virtual Machine - Name, Type, and Version](images/vbox2a.png)

Enter a descriptive name for the virtual machine.
Open the **Operating System** drop-down menu and select *BSD*.
In the **Version** drop-down menu, select *FreeBSD (64 bit)*.
Click *Next*.

![Virtual Machine Reserved Memory](images/vbox3a.png)

The base memory size must be changed to **at least 2048 MB.**
Assigning more RAM improves the guest operating system performance.
Any number within the green area is considered a "safe" value by VirtualBox and should not impact the host computer performance.
When finished, click *Next*.

![Virtual Hard Drive - New or Existing](images/vbox4a.png)

This section is for allocating host computer disk space to the guest operating system or creating a virtual hard drive.
Using the default choices is generally recommended.
Click *Create* to configure the virtual hard disk.
An existing virtual disk can be reused by selecting *Use an existing virtual hard drive file* from the drop-down menu.
Create as many virtual drives as desired.
Consider reusing existing virtual hard drives to save space on the physical hard drive.

![Hard Drive Type](images/vbox5a.png)

Select *VDI* and click *Next*.

![Storage Type](images/vbox6a.png)

Choose whether to have *Dynamically allocated* or *Fixed size* storage.
*Dynamically allocated* uses disk space as needed until it reaches a specified maximum size.
The *Fixed size* option reserved physical space from the physical hard disk, regardless if the virtual machine uses the space.
Choose *Dynamically allocated* when physical disk space is a concern.
Choose *Fixed size* when space is not a concern, as it allows the virtual machine to run slightly faster.
Click *Next*.

![Virtual Disk - File Name and Size](images/vbox7a.png)

Set the virtual disk size or upper limit.
When installing Project Trident as the virtual machine guest OS, set the size to at least **20 GB**.
Set the size to a minimum **50 GB** when planning to use the virtual machine more extensively.
Whatever size is set, be sure the computer has enough free disk space to accommodate the virtual disk size.
Use the folder icon to choose a directory with sufficient space to hold the virtual disk.

Click *Create* to finish the process and return to the main screen.

![New Virtual Machine "Trident-VM"](images/vbox8a.png)

Configure a bridged network to enable internet access for the virtual machine.
Right-click the virtual machine and go to Settings --\> Network.
Open the **Attached to** drop-down menu and select *Bridged Adapter*.
Select the name of the physical network interface from the **Name** drop-down menu.
In the example image, the *Intel Pro/1000 Ethernet card* is attached to the network and is named *re0*.

![VirtualBox Bridged Adapter Example Configuration](images/vbox9a.png)

Before starting the virtual machine, configure it to use the ISO installation media previously downloaded.
Click *Storage* hyperlink in
the right frame to access the Storage screen seen in
Figure %s \<vbox10\>.

![Virtual Machine Storage Settings](images/vbox10a.png)

Click the word Empty, which represents the DVD reader. To access the
Project Trident installer from the DVD reader, double-check the Slot is
pointing to the correct location (e.g. IDE Secondary Master) and use the
drop-down menu to change the location if incorrect.

If using an ISO stored on the hard disk is preferred, click the DVD icon
then Choose a virtual CD/DVD disk file to open a browser menu to
navigate to the location of the ISO. Highlight the desired ISO and click
Open. The name of the ISO will now appear in the Storage Tree section.

Project Trident is now ready to be installed into the virtual machine as a
guest OS. Highlight the virtual machine and click on the green Start
icon. A new window opens, indicating the virtual machine is starting. If
a DVD is inserted, it should audibly spin and the machine will start to
boot into the installation program. If it does not or if using an ISO
stored on the hard disk, press F12 to select the boot device when the
message to do so appears, then press c to boot from CD-ROM. Proceed
through the installation as described in the Install section.

> **note**
>
> If the installer GUI doesn't appear to load after configuring the
> virtual machine, **EFI** may need to be enabled in Virtualbox by
> navigating Settings --\> System --\> Motherboard and checking
> Enable EFI (special OSes only).

Supported Hardware
------------------

While the Project Trident installer is very easy to use, installing a brand new
operating system can sometimes be a daunting task.

Before beginning, there are a few things to check to ensure the system
is ready to install Project Trident.

-   **Dual-booting or installing over the entire drive?** If
    dual-booting, please ensure a primary partition is available. Refer
    to the chapter on Dual Booting.
-   **Ensure important data is backed up!** Any irreplaceable data, such
    as emails, bookmarks, or important files and documents should
    **always** be backed up to an external media, such as a removable
    drive or another system, **before** installing or upgrading any
    operating system. Accidents happen, and losing important data can be
    avoided.

To determine if the chosen hardware is detected by Project Trident, start a new
installation and click the Hardware Compatibility icon in the lower left
corner of the Language screen.

If any problems arise with the installation, refer to the
Troubleshooting section of this handbook.

Hardware Requirements and Supported Hardware
--------------------------------------------

This section discusses the Project Trident hardware requirements and some
supported hardware.

### Minimum Requirements

Project Trident has moderate hardware requirements and typically uses less
resources than its commercial counterparts. Before installing Project Trident,
make sure the hardware or virtual machine meets at least the minimum
requirements. To get the most out of the Project Trident experience, use a
system exceeding the minimum or recommended system requirements.

At a **bare minimum**, these requirements must be met in order to
install Project Trident:

**Minimum Requirements**

-   64-bit processor
-   1 GB RAM
-   10 - 15 GB of free hard drive space on a **primary partition** for a
    command-line server installation.
-   Network card

Here are the **recommended** requirements. More RAM and available disk
space improves the computing experience:

**Recommended Requirements**

-   64-bit processor
-   4 GB of RAM
-   20 - 30 GB of free hard drive space on a primary partition for a
    graphical desktop installation.
-   Network card
-   Sound card
-   3D-accelerated video card

Project Trident does not require 50 GB for its installation. The minimum
recommendation is to provide sufficient room for the installation of
applications and to store local ZFS snapshots and boot environments.
These can be used to retrieve earlier versions of files, rollback the
operating system to an earlier point in time, or clone the operating
system.

There is no such thing as too much RAM. ZFS thrives on systems with lots
of RAM. To play modern video games, use a fast CPU. To create a
collection of music and movies on the computer, sufficient disk space is
required.

### Processor

Project Trident installs on any system containing a 64-bit (also called
*amd64*) processor. Despite the name, a 64-bit processor does **not**
need to be manufactured by AMD in order to be supported. Even 64-bit
Intel CPUs are sometimes referred to as amd64. The [FreeBSD Hardware
Notes -
amd64](https://www.freebsd.org/releases/11.0R/hardware.html#proc-amd64)
lists the *amd64* processors known to be compatible.

### Graphics

Like many open source operating systems, Project Trident uses
[X.org](https://www.x.org/wiki/) drivers for graphics support. Project Trident
automatically detects the optimal video settings for supported video
drivers. Verify the graphics hardware is supported by clicking the
Hardware Compatibility icon within the installer.

Here are the major graphic vendors supported in Project Trident:

**NVIDIA:** 3D acceleration on NVIDIA is provided by native FreeBSD
drivers. If an NVIDIA video card is detected, an nVidia settings icon
will be added to Browse Applications for managing NVIDIA settings.

**Intel:** 3D acceleration on most Intel graphics is supported. This
includes Skylake, Haswell, Broadwell, and ValleyView chipsets.

**ATI/Radeon:** 3D acceleration on most ATI and Radeon cards is
supported.

**Optimus:** Currently, there is no switching support between the two
graphics adapters provided by Optimus. Optimus implementations vary, so
Project Trident may or may not be able to successfully load a graphics driver
to support this hardware. If a blank screen shows after installation,
check the BIOS to see if there is an option to disable one of the
graphics adapters or to set *discrete* mode. If the BIOS does not
provide a *discrete* mode, Project Trident defaults to the 3D Intel driver and
disables NVIDIA. This will change in the future when the NVIDIA driver
supports Optimus.

### Wireless

Project Trident has built-in support for most wireless networking cards.
Project Trident automatically detects available wireless networks for supported
wireless devices. Verify the device is supported by clicking the
Hardware Compatibility icon within the installer. If it is an external
wireless device, insert it before running the installer.

Certain Broadcom devices, typically found in less expensive laptops, are
buggy and lockup unexpectedly while in *DMA* mode. If the device
freezes, try switching to *PIO* mode in the BIOS. Alternately, add
hw.bwn.usedma=0 to /boot/loader.conf and reboot to see if that resolves
the issue.

> **note**
>
> Some wifi adapters are not configured during installation,
> :   but after first boot. Just because an adapter does not show up
>     during installation does not mean it is unsupported.
>
### Laptops

Many Project Trident users successfully run Project Trident on their laptops. However,
some issues may occur, depending upon the model of laptop. Some typical
laptop issues:

-   **Sleep/suspend:** Unfortunately,
    Advanced Configuration and Power Interface \<Advanced\_Configuration\_and\_Power\_Interface\>
    (ACPI) is not an exact science, meaning experimentation with various
    sysctl variables may be required to achieve successful sleep and
    suspend states on some laptops. For ThinkPad laptops, the
    [ThinkWiki](http://www.thinkwiki.org/wiki/ThinkWiki) is an excellent
    resource. For other types of laptops, try reading the *SYSCTL
    VARIABLES* section of man 4 acpi and check to see if there is an
    ACPI man page specific to the laptop's vendor by typing
    apropos acpi. The [Tuning with
    sysctl(8)](https://www.freebsd.org/doc/en/books/handbook/configtuning-sysctl.html)
    section of the FreeBSD Handbook demonstrates how to determine the
    current sysctl values, modify a value, and make a modified value
    persist after a reboot.
-   **Synaptics:** Disabling the system's touchpad may be dependant upon
    the hardware. This [forum
    post](https://forums.freebsd.org/threads/17370/#post-100670)
    describes how to enable Synaptics and some of the sysctl options
    this feature provides.

    The [SysAdm Mouse Settings](https://sysadm.us/handbook/client/) also
    has options for disabling a system's touchpad, if one is detected.

To test the laptop's hardware, use the Hardware Compatibility icon in
the Language screen before continuing with the installation.

To install Project Trident onto an Asus Eee PC, review the [FreeBSD Eee
page](https://wiki.FreeBSD.org/AsusEee) first.

The FreeBSD [Tuning Power Consumption
page](https://wiki.FreeBSD.org/TuningPowerConsumption) has some tips for
reducing power consumption.

With regards to specific hardware, the ThinkPad T420 may panic during
install. If it does, enter the BIOS and set the video mode to
"discrete", which should allow the installation to complete. Some
Thinkpads have a BIOS bug preventing them from booting from GPT-labeled
disks. If unable to boot into a new installation, restart the installer
and go into Advanced Mode in the Disk Selection screen. Make sure
GPT (Best for new hardware) is unchecked. If it was checked previously,
redo the installation with the box unchecked.
