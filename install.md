Install
=======

This chapter describes how to use the graphical installer to install a
graphical desktop directly onto a hard drive or into a virtual machine
using virtualization software such as
[VirtualBox](https://www.virtualbox.org/).

Download and Prepare to Install
-------------------------------

|trident| uses a rolling release model rather than versioned releases.
There are two primary options of |trident| install: STABLE and UNSTABLE:

1.  STABLE is synchronized with FreeBSD. This means users see less
    experimental work and generally have a smoother experience. However,
    users on STABLE typically wait longer for bugfixes and patches to be
    available. While some |trident| development may be backported to
    STABLE early, FreeBSD patches and port synchronization is done on a
    six-month schedule.
2.  UNSTABLE is the full leading edge of trident and FreeBSD development.
    Patches are very frequent, but can incorporate experimental work
    from |trident|, FreeBSD, and other Open Source projects and
    contributions. UNSTABLE is recommended for users who need the
    absolute latest work from FreeBSD or |trident| and are willing to
    tolerate breakage or less system stability. It is also recommended
    for users who want to test and contribute patches to FreeBSD or
    |trident|.

Periodically, the |sysadm| Update Manager \<update-manager\> provides
patches to update the operating system. By default, users who install
STABLE receive updates from the STABLE track, and UNSTABLE users follow
the UNSTABLE track. It is possible to switch update tracks
post-installation. See the Updating trident section for instructions on
switching update repositories.

Installation files can be downloaded from the [trident
website](https://www.trident.org/downloads/).

Figure %s \<downloadscreen1\> below shows the |trident| website, and how
to download a STABLE or UNSTABLE version of |trident|. It also shows a
drop down menu containing the different types of install files available
for download.

![UNSTABLE or STABLE Download Screen](images/downloadscreen1.png)

To install a graphical desktop, download the |trident| Desktop option.
Then, depending on the file chosen, either burn it to a DVD media or
write it to a removable USB device.

If installing a command-line only server is preferred, download and
begin installing the |trident| Server option.

Install files can end with a variety of extensions:

-   **.iso**: If the file has an *.iso* extension, it should be burned
    to a DVD media.
-   **.img**: If it has a *img* extension, it should be burned to a USB
    stick.
-   **.md5, .sha256, and .sig**: Depending upon the current operating
    system and its tools, use the value in any of these files to
    determine the integrity of the download, as described in
    Data Integrity Check.
-   **.torrent**: If a torrent is available, a file with the same name
    and a *.torrent* extension will be visible.

Refer to Burning the Installation Media for instructions on how to burn
the downloaded file to bootable media.

### Data Integrity Check

After downloading the desired file, it is a good idea to check the file
is exactly the same as the one on the |trident| download server. While
downloading, a portion of the file may get damaged or lost, making the
installation file unusable. Each |trident| installation file has an
associated *MD5* and *SHA256* checksum. If a checksum of the downloaded
file matches, the download was successful. If a checksum does not match,
try downloading the file again. In order to verify a checksum, use a
checksum verification utility.

> **note**
>
> Only one of the checksums needs to be verified. The [trident
> website](https://www.trident.org/downloads/) lists *.MD5*, *SHA256*,
> and *.SIG* files. The [trident
> website](https://www.trident.org/downloads/) has all file types.

If using a Windows system, download and install a utility such as
[Raymond's MD5 & SHA Checksum
Utility](http://download.cnet.com/MD5-SHA-Checksum-Utility/3000-2092_4-10911445.html).
This utility can be used to simultaneously check the *MD5*, *SHA-1*,
*SHA-256*, and *SHA-512* checksums of any file. Once installed, launch
the program and use Browse, shown in Figure %s \<fastsum1\>, to browse
to the location of the downloaded file.

![Checksum Verification](images/checksum.png)

Once the file is selected, click Open to calculate the checksums. It may
take a minute or so, depending upon the size of the downloaded file.

On Linux and BSD systems, use the built-in md5 or md5sum command line
tool to display the MD5 checksum. In this example, the user types md5 to
view the sum of a .img file located in the Downloads directory. Then,
using the built-in cat command line tool, the user compares the sum to
the contents of the related .md5 file:

``` {.sourceCode .none}
~% md5 Downloads/trident-2017-04-21-x64-USB.img
MD5 (Downloads/trident-2017-04-21-x64-USB.img) =
3eb6adef0ad171f6c5825f0f820557f5

~& cat Downloads/trident-2017-04-21-x64-USB.img.md5
3eb6adef0ad171f6c5825f0f820557f5
```

To use the *OpenPGP* .sig file, use your preferred utility to verify the
signature. The [OpenPGP website](https://openpgp.org/) has numerous
recommendations for verification utilities.

### Burning the Installation Media

Once the installation file is downloaded and its checksum verified, burn
it to a media. The media you use depends upon the file downloaded:

-   Files ending with .iso can be burned to a DVD or used in a Virtual
    Machine (VM).
-   Files ending in img must be burned to a USB stick.

To burn to a DVD, use either a burning utility packaged with the
operating system on the system with the burner or a separate burning
application. Table %s \<burn utils\> lists some freely available burning
utilities.

### Writing to a USB Device

There are a few requirements to write the img file to a USB device:

-   A utility capable of writing the image to a USB media; the available
    utilities depend on the installed operating system.
-   A USB thumb drive or hard drive large enough to hold the image.

> **warning**
>
> If there is a card reader on the system or the USB drive is connected
> using a USB dongle, device enumeration may be affected. For example,
> with the USB card reader dongle as the destination, the device name
> could be /dev/da1 instead of /dev/da0.

To write the .img file to a flash card or removable USB drive on a BSD
or Linux system, use the dd command line utility. On a FreeBSD system,
the superuser can use this command to write the file to the first
plugged in USB device:

``` {.sourceCode .none}
[user@exmpl] dd if=trident-Desktop-2016-08-11-x64.img of=/dev/da0 bs=1m
1415+1 records in
1415+1 records out
1483990016 bytes transferred in 238.552250 secs (6220818 bytes/sec)
```

When using the dd command:

-   **if=** designates the *input file* to be written.
-   **of=** refers to the *output file* (the device name of the flash
    card or removable USB drive). Increment the number in the name if it
    is not the first USB device.
-   **bs=** refers to the *block size*.

> **note**
>
> On Linux, type mount with the USB stick inserted to see two or more
> device nodes corresponding to the USB stick. For example, /dev/sdc and
> /dev/sdc1, where /dev/sdc1 corresponds to the primary partition of the
> USB stick. Before using dd, ensure the USB stick is unmounted. Then,
> remember to use /dev/sdc (the device node without the number) as the
> option for the output file **of=**. Once dd completes, the USB stick
> may not be mountable on Linux as it has very limited support for UFS
> (BSD filesystem created on the USB stick).

To burn the image file on a Windows system, use
[win32-image-writer](https://sourceforge.net/projects/win32diskimager/).
When downloading **win32-image-writer**, download the latest version
ending in -binary.zip and use a utility such as Windows Explorer or 7zip
to unzip the executable.

Launch win32-image-writer.exe to start the Win32 Disk Imager utility,
shown in Figure %s \<writer1\>. Use browse to browse to the location of
the .img file. Insert a USB thumb drive and select its drive letter (in
this example, drive **D**). Click Write and the image will be written to
the USB thumb drive.

![Write an Image using Win32 Disk Imager](images/writer1.png)

To burn the .img file on Mac OS X, insert a USB stick and open
*Terminal*. Run diskutil list to discover the device name of the USB
disk, unmount the USB disk, then use dd to write the image to the raw
disk (rdisk). In this example, an 8 GB USB stick has a device name of
/dev/disk1 and a raw device name of /dev/rdisk1:

``` {.sourceCode .none}
diskutil list
/dev/disk0
#: TYPE NAME SIZE IDENTIFIER
0: GUID_partition_scheme *500.1 GB disk0
1: EFI 209.7 MB disk0s1
2: Apple_HFS Macintosh HD 499.2 GB disk0s2
3: Apple_Boot Recovery HD 650.0 MB disk0s3
/dev/disk1
#: TYPE NAME SIZE IDENTIFIER
0: FDisk_partition_scheme *8.0 GB disk1
1: DOS_FAT_32 UNTITLED 8.0 GB disk1s1

diskutil unmountDisk /dev/disk1
Unmount of all volumes on disk1 was successful

sudo dd if=/Users/dru/Downloads/trident-Desktop-2016-08-11-x64.img of=/dev/rdisk1 bs=4m
Password:
1415+1 records in
1415+1 records out
1483990016 bytes transferred in 238.552250 secs (6220818 bytes/sec)
```

|trident| Installation
---------------------

To begin the |trident| installation, insert the prepared boot media and
boot the system. If the computer boots into an existing operating system
instead of the installer, reboot and check the computer's BIOS program
to ensure the drive containing the installation media is listed first in
the boot order. Save any BIOS changes and reboot.

Once the system boots it displays the menu shown in
Figure %s \<install1\>. Press Enter or simply wait a few moments and
this menu automatically prompts the system to continue booting.

![Initial Boot Menu](images/install1b.png)

> **note**
>
> See BSD Boot Loader for a detailed description of this menu.

If a key other than Enter is pressed, this screen pauses to provide
additional time to review the options. If this screen is not paused, it
automatically boots into the Boot Multi User option, displaying the
first graphical installer screen, shown in the Language install section.

The rest of this chapter describes the screens of the graphical
installer. If any problems arise with booting into the graphical
installer, please refer to the
Installation Troubleshooting \<Installation Help\> section of this
handbook.

Language
--------

The first graphical installer screen, seen in Figure %s \<install2\>,
indicates the installer successfully loaded and is ready to present its
options.

![Welcome and Language Selection Screen](images/install2e.png)

On the bottom-left side of the screen are several icons and buttons to
help with the installation, explained in Table %s \<insico\>:

Hover over an icon to view its description in the tip bar at the bottom
of the screen.

> **note**
>
> The default keyboard layout can be changed at this point, during the
> post-installation Choose a Language screen, when Logging In, or during
> an active session using the included fcitx utility.

There is also an option to Load config from USB. If the configuration
from a previous installation has been saved, it can be loaded at this
time from a *FAT* formatted USB stick.

By default, |trident| menus display in English, unless another language
is selected in the drop-down menu in this screen. The menus in |trident|
are being continuously translated to other languages. To view the
availability of a specific language, navigate to the [trident
Translation Site](https://weblate.trident.org). A language may show less
than 100% translation, indicating not all of the menus are translated.
Any untranslated menus are displayed in English. Refer to
Become a Translator to assist in translating the graphical menus.

> **note**
>
> Small screens may not display the entire installer window, resulting
> in buttons at the bottom of the window being hidden and inaccessible.
> In this situation, either press Alt while dragging the window with the
> mouse or press Alt+N to select the next button of the window.

When finished reviewing this screen, click Next to move on to the next
installation screen.

System Selection
----------------

The **System Selection** screen installs a graphical desktop or a
console-based server operating system, as seen in
Figure %s \<install3\>. It also can be used for
Restoring the Operating System. This chapter concentrates on a desktop
installation. Refer to the Server Installation instructions for
installing a command-line only server.

![System Selection Screen](images/install3e.png)

By default, trident Desktop (graphical interface) is selected. The
|lumina| Desktop is installed with trident, but additional software can
be installed later using AppCafe \<appcafe\>.

To install the desktop, click Next.

> **note**
>
> When installing to an existing |pcbsd| or |trident| system, a pop-up
> window asks to install to the existing pool without reformatting it.
> Press OK to keep the existing pool. Clicking Cancel formats the
> existing pool and all of its data. Refer to the
> Upgrading from PCBSD 10.x to trident section for more information about
> this option.

Optional Packages
-----------------

By default, |trident| loads only two graphics drivers during the
installation: VESA (for MBR) and SCFB (for UEFI). |trident| provides the
option to further choose your graphics driver as part of the
Figure %s \<install16\> screen.

![Optional Installation Packages](images/install16.png)

When installing |trident|, it detects the onboard graphics solution and
displays a list of drivers you can use for |trident|. Additionally,
VirtualBox is automatically detected, populating the list with *Virtual
Environment Drivers*.

Expand the desired list of drivers and choose one which is compatible
with your hardware, then click Next to continue.

Disk Selection
--------------

The **Disk Selection** screen, seen in Figure %s \<install5\>,
summarizes the default disk configuration.

![Disk Selection Screen](images/install5d.png)

> **warning**
>
> By default, |trident| assumes the user wants to install on the entire
> first disk. When installing |trident| as the only operating system on
> the computer, click Next to start the installation. However, if this
> is not intended, review the rest of this section to determine how to
> layout the disk. If |trident| is to be booted with another operating
> system, please review the section on Dual Booting.

To select the disk or partition to install |trident|, click
Customize Disk Settings to start the |trident| Disk Wizard, shown in
Figure %s \<install6\>.

![|trident| Disk Wizard](images/install6c.png)

The wizard provides two modes of operation:

-   **Basic:** (default) Select this mode if to specify the installation
    partition or disk.
-   **Advanced:** Select this mode to specify the installation partition
    or disk, use MBR partitioning, change the default ZFS pool name,
    force the block size used by ZFS, configure a multi-disk
    installation, add a log or cache device, encrypt the disk, or
    specify the filesystem layout.

> **warning**
>
> Regardless of the selected mode, once the disk wizard completes and
> Next is chosen at the **Disk Selection** screen, a pop-up window asks
> to start the installation. Be sure to review the **Summary** area
> before clicking Yes and starting the installation. The **Disk
> Selection** screen is the **very last chance** to ensure the system is
> correctly configured. After clicking Yes, the selected hard drive or
> partition is formatted, losing any existing data.

Once finished configuring the disk, you can save your choices for later
use. Insert a FAT32 or MSDOSFS formatted USB stick and click
Save Config to USB.

### Basic Mode

Select Basic and the wizard displays the screen shown in
Figure %s \<install7\>.

![Disk or Partition Selection](images/install7c.png)

The first hard disk is typically selected. To install on a different
disk, use the Disk drop-down menu to select the install disk.

By default, the entirety of the selected disk is formatted. If the disk
is divided into partitions or there is an area of free space, use the
Partition drop-down menu to choose the desired partition.

> **note**
>
> |trident| only installs into a primary MBR partition, a GPT partition,
> or an area of free space. |trident| cannot install into a secondary or
> an extended partition. To create an area of free space for
> installation, refer to Creating Free Space.

For EFI/UEFI systems, you can choose to Install rEFInd. The [rEFInd boot
manager](http://www.rodsbooks.com/refind/) is used to provide a menu of
boot options to the user when the computer boots. It is required by
|trident| when Dual Booting.

> **note**
>
> rEFInd is a boot manager which functions separately from the FreeBSD
> bootloader.

Once the disk and partition are selected, click Next to view a
**Summary** screen to review your choices. To make additional changes,
press Back to return to a previous screen. Otherwise, click Finish to
leave the wizard. Click Next then Yes to start the installation.

### Advanced Mode

After selecting advanced mode, the wizard displays the screen shown in
Figure %s \<install8\>.

![Advanced Mode Options](images/install8d.png)

This screen has several options:

-   **Disk:** Choose the install disk.
-   **Partition:** Select the desired partition or area of free space.

> **note**
>
> |trident| onlys install into a primary MBR partition, a GPT partition,
> or an area of free space. |trident| cannot install into a secondary or
> an extended partition. To create an area of free space for
> installation, refer to Creating Free Space.

-   **Partition Scheme:** The default GPT (Best for new hardware) is a
    partition table layout supporting larger partition sizes than the
    traditional MBR (Legacy) layout. **If the installation disk or
    partition is larger than 2 TB, the GPT option must be selected**.
    Since some older motherboards do not support GPT, if the
    installation fails, try again with MBR (Legacy) selected. When in
    doubt, use the default selection.

> **note**
>
> The **Partition Scheme** section does not appear if a partition other
> than Use entire disk is chosen in the Partition drop-down menu.

-   **ZFS pool name:** To use a pool name other than *tank* (default),
    check this box and type the name of the pool in the text window.
    *Root* is reserved and can not be used as a pool name.
-   **Force ZFS 4k block size:** This option is only used if the disk
    supports 4k, even though the disk may lie and report its size as
    512b. Use with caution as it may cause the installation to fail.
-   **Install rEFInd:** For EFI/UEFI systems, you can choose to
    Install rEFInd. The [rEFInd boot
    manager](http://www.rodsbooks.com/refind/) is used to provide a menu
    of boot options to the user when the computer boots. It is required
    by |trident| when Dual Booting.

After making any selections, click Next to access the ZFS configuration
screens. The rest of this section provides a ZFS overview and then
demonstrates how to customize the ZFS layout.

#### ZFS Layout

In Advanced Mode, the disk setup wizard allows configuring the ZFS
layout. The initial ZFS configuration screen is seen in
Figure %s \<install9\>.

![ZFS Configuration](images/install9c.png)

If the system contains multiple drives to be used to create a ZFS mirror
or RAIDZ\*, check Add additional disks to storage pool, which enables
this screen. Any available disks are listed in the box below the
ZFS Virtual Device Mode drop-down menu. Select the desired level of
redundancy from the ZFS Virtual Device Mode drop-down menu, then check
the box for each disk to add to the configuration.

> **note**
>
> The |trident| installer requires entire disks (not partitions) when
> adding more disks to the pool.

While ZFS allows using disks of different sizes, this is discouraged as
it decreases storage capacity and ZFS performance.

The |trident| installer supports multiple ZFS configurations:

-   **mirror:** Requires a minimum of 2 disks.
-   **RAIDZ1:** Requires a minimum of 3 disks. For best performance, a
    maximum of 9 disks is recommended.
-   **RAIDZ2:** Requires a minimum of 4 disks. For best performance, a
    maximum of 10 disks is recommended.
-   **RAIDZ3:** Requires a minimum of 5 disks. For best performance, a
    maximum of 11 disks is recommended.
-   **stripe:** Requires a minimum of 2 disks.

> **danger**
>
> A stripe does NOT provide ANY redundancy. If any disk fails in a
> stripe, all data in the pool is lost!

The installer does not allow a configuration choice in which the system
does not meet the required number of disks. When selecting a
configuration, a message indicates how many more disks are required.

When finished, click Next to choose cache and log devices, shown in
Figure %s \<install10\>.

![L2ARC and ZIL](images/install10b.png)

This screen can be used to specify an SSD as an L2ARC read cache or as a
secondary log device (ZIL). Any available devices are listed in the
boxes in this screen.

> **note**
>
> A separate SSD is needed for each type of device.

Refer to the descriptions for ZIL and L2ARC in the ZFS Overview to
determine if the system would benefit from any of these devices before
adding them in this screen. When finished, click Next to move to the
encryption options, shown in Figure %s \<install11\>.

![Encryption](images/install11d.png)

This screen can be used to configure full-disk encryption. This is meant
to protect the data on the disks should the system itself be lost or
stolen. This type of encryption prevents the data on the disks from
being available during bootup unless the correct passphrase is typed at
the bootup screen. Once the passphrase is accepted, the data is
unencrypted and can easily be read from disk.

To configure full-disk encryption, check Encrypt disk with GELI. This
option will be greyed out if GPT (Best for new hardware) is not selected
as GELI does not support MBR partitioning. If needed, use Back to go
back to the Advanced Mode screen and select GPT (Best for new hardware).
Once that box is checked, input a strong passphrase twice into the
Password fields. It is recommended to create a long and memorable
password, but something difficult to guess.

> **danger**
>
> This passphrase is required to decrypt the disks. If the passphrase is
> lost or forgotten, all access will be lost to the encrypted data!

When finished, click Next to move to the mount point screen shown in
Figure %s \<install12\>.

![Default ZFS Layout](images/install12c.png)

Regardless of how many disks are selected for the ZFS configuration, the
default layout is the same. ZFS does not require separate partitions for
/usr, /tmp, or /var. Instead, create one ZFS partition (pool) and
specify a mount for each dataset. A /boot partition is not mandatory
with ZFS as the |trident| installer puts a 64k partition at the beginning
of the drive.

> **warning**
>
> Do not remove any of the default mount points. These are all used by
> |trident|.

Use Add to add additional mount points. The system will ask for the name
of the mount point as size is not limited at creation time. Instead, the
data on any mount point can continue to grow as long as space remains
within the ZFS pool.

To set the swap size, click Swap Size. This prompts you to enter a size
in MB. If a RAIDZ\* or mirror exists, a swap partition of the specified
size is created on each disk and mirrored between the drives. For
example, if a 2048 MB swap size is specified, a 2 GB swap partition is
created on all the specified disks, but the total swap size is 2GB
because of redundancy.

Right-click any mount point to toggle between enabling or disabling many
ZFS properties:

-   **atime:** When set to on, controls whether the access time for
    files is updated when they are read. When set to off, this property
    avoids producing write traffic when reading files. This can result
    in significant performance gains, though it may confuse mailers and
    other utilities.
-   **canmount:** If set to off, the filesystem is unmountable.
-   **casesensitivity:** The default is sensitive, as UNIX filesystems
    use case-sensitive file names. For example, "kris" is different from
    "Kris". To tell the dataset to ignore case, select insensitive.
-   **checksum:** Automatically verifies the integrity of the data
    stored on disks. Turning this property off is highly discouraged.
-   **compression:** If set to on, automatically compresses stored data
    to conserve disk space.
-   **exec:** If set to off, processes can not be executed from within
    this filesystem.
-   **setuid:** If set to on, the set-UID bit is respected.

After clicking Next, the wizard shows a summary of the selections. To
make further changes, use Back to return to a previous screen.
Otherwise, click Finish to leave the wizard and return to the
Disk Selection screen.

Installation Progress
---------------------

Once Yes is selected to start the installation, a progress screen, seen
in Figure %s \<install13\>, updates the user on the installation
progress.

![Installation Progress](images/install13c.png)

How long the installation takes depends upon the speed of the hardware
and the installation type selected. A typical installation takes between
5 and 15 minutes.

Installation Finished
---------------------

The **Installation Finished** screen, shown in Figure %s \<install14\>,
appears once the installation is complete.

![|trident| Installation Complete](images/install14c.png)

Click Finish to complete the |trident| installation. The system
immediately begins the reboot process. Once the system is fully shut
down, remove the installation media to ensure the system boots from the
freshly installed local drive.

Booting Into |trident|
---------------------

After installation, |trident| reboots and displays a boot menu. The first
menu displayed depends on whether or not rEFInd is installed or the user
customized the boot loader during the installation.

### rEFInd Boot Manager

For EFI or UEFI systems, the user can choose to install rEFInd. This is
a boot manager that is useful when Dual Booting. Figure %s \<refind1.2\>
shows the initial rEFInd screen.

![rEFInd Boot Manager](images/refind1.png)

rEFInd displays any installed operating systems, booting into the
default choice after a few seconds. Press any key other than Enter to
pause automatic booting, then use the arrow keys to select the desired
operating system. Press Enter to continue booting.

There are a number of options in rEFInd aside from choosing an operating
system:

-   **About rEFInd:** This option displays the version and copyrights of
    rEFInd. It also shows the EFI Revision, Platform, Firmware, and
    Screen Output.
-   **Shut Down Computer**
-   **Reboot Computer**
-   **Reboot to Computer Setup Utility:** Not recommended for use with
    |trident|.

Additional boot options for an operating system are available by
highlighting the OS and pressing F2 or Insert.

Once |trident| is chosen in rEFInd, the next boot screen displays.

### BSD Boot Loader

A system with a default or "BSD" install option for the boot loader
loads the boot menu seen in Figure %s \<install4\>.

> **note**
>
> This menu is modified from the one seen when booting into the
> installer \<install1\>. While the options are the same, they are
> rearranged slightly to prevent confusion and unnecessary clutter.

![|trident| Boot Menu](images/install4.png)

This menu provides several options. Pause this menu by pressing any key
except for Enter. To select an option, press either the bolded number or
key for that option. Once any selections are made, press Enter to boot
using the specified options.

-   1. Boot trident [Enter]: This is the default option for booting
    |trident|. The system automatically uses this option either after
    pausing for a moment or if Enter is pressed while the boot menu is
    displayed.
-   2. Configure Boot Options: Press either 2 or o to see the boot
    options screen, shown in Figure %s \<boot1\>. To change an option,
    press either the bolded number or key for the option to toggle
    through its available settings. When finished, press either 1 or
    Backspace to return to the |trident| boot menu.
-   3. Select Boot Environment: In |trident|, boot environments are
    automatically created when the system updates. They can also be
    manually created using the
    Boot Environment Manager \<boot-environment-manager\>. This allows
    the system to boot to the point of time before an update occurred
    and can be used to recover from a failed update. Press either 3 or e
    to view the available boot environments.

> **tip**
>
> The first time the system boots, no additional environments are
> available. This menu populates as boot environments are created.

![Boot Options Menu](images/boot1c.png)

Several boot options are available in the Boot Options Menu:

-   3. Boot Single User: Advanced users can select this option to fix
    critical system failures.
-   4. Verbose: Select this option to see more detailed messages during
    the boot process. This can be useful when troubleshooting a piece of
    hardware.
-   5. Kernel: This option indicates how many kernels are available.
    Press either 5 or k to toggle between available kernels. This option
    is available to the user if they have created a custom kernel, but
    wish to have a kernel.old boot option available in case the custom
    primary kernel fails.
-   6. Escape to loader prompt: Advanced users can select this option to
    perform advanced operations, such as loading kernel modules.

### Encrypted Disks

If Encrypt disk with GELI was selected during installation, physical
access to the |trident| system when it boots is required. As the system
starts to boot, it displays a message similar to the one shown in
Figure %s \<encrypt1\>.

![Master Key Decryption](images/encrypt1.png)

The boot process will wait for the password created in the installation
screen shown in Configure Encryption \<install11\>. If the correct
password is typed, the system calculates the GELI encryption key then
continues to boot.

Display Detection
-----------------

When booting for the first time, |trident| shows a Display Settings
screen, reproduced in Figure %s \<display3\>.

![Display Settings](images/display3a.png)

Use this screen to view the detected video card and choose a graphics
driver from the expanding menu. |trident| also suggests a driver.

The vesa driver always works but provides sub-optimal performance. Click
on the drop-down menu to select the driver most closely matching your
video card name.

When finished, click Apply for the settings to be tested. If anything
goes wrong during testing, the system returns to the Display Settings
screen in order for the user to select another driver. Once satisfied
with the settings, click Yes when prompted to accept them.

> **note**
>
> The Advanced tab is disabled and scheduled for removal.

Choose a Language
-----------------

Figure %s \<config1\> shows the language selection screen.

![Language Selection](images/config1a.png)

This allows for the selection of the language used to access the
installed system. It also contains three icons from the installer
screens to enable:

-   **Light Bulb**: Reading the screen's *Help* text.
-   **Keyboard**: Use the onscreen keyboard.
-   **Key with US and Brazilian Flag**: Choose a different keyboard
    layout other than the default US style.

Once the selection is made, click Next to move to the next configuration
screen.

Time Zone Selection
-------------------

The timezone select screen, shown in Figure %s \<config2\>, allows
selection of the timezone and configuring the system's host and domain
names.

![Time Zone Selection](images/config2c.png)

Use the drop-down menu to select the city closest to the system's
location. If the system is connected to the Internet, the installer
automatically attempts to detect the correct timezone.

If the system is dual booting and the other operating system expects the
BIOS to use UTC, also check Set BIOS to UTC time.

A default system hostname is created. Change the name by typing the
desired hostname in the System Hostname field. If the computer is a
member of a DNS domain, the Domain Name is also an option.

When finished, click Next to proceed to the next screen.

Set the Root Password
---------------------

This screen, seen in Figure %s \<config3\>, **requires** setting the
root (administrative) password.

![Root Password Creation](images/config3b.png)

The password must be a minimum of **4** characters and typed twice to
confirm the password. Try to create a complex, but memorable password,
as this is used whenever the system indicates administrative access is
required. Click Next when finished.

Create a User
-------------

This screen is used to create the primary user account used to login to
the system.

Figure %s \<config4\> shows the configuration screen used to create the
initial user account.

![User Creation](images/config4b.png)

The User Details tab is used to create a login user. This screen
requires completing several fields:

-   **Name:** This value displays in the login screen. It can be the
    user's full name and can contain both capital letters and spaces.
-   **Username:** This is the name used when logging in. It can **not**
    contain spaces and **is** case sensitive (e.g. *Kris* is a different
    username from *kris*).
-   **Password:** This is the password to use when logging in. It must
    be typed twice for confirmation.
-   **Specify UID:** By default, the user is assigned the next available
    User ID (UID). If a specific UID is required, it can be set here. A
    UID can not be set lower than 1001, and a UID already in use by
    another account is also unavailable.

|trident| provides the ability to use a removable device, such as a USB
stick, as the user's encrypted home directory. This is useful in a
multi-user or multi-computer environment, as it provides the user with
secure access to their encrypted files. When a user initializes
PersonaCrypt \<personacrypt\> with their account, their username only
appears in the login menu if the removable media associated with that
|trident| system is inserted. They must input the password associated
with the removable device in order to log in.

When a user is configured to use a PersonaCrypt device, that user cannot
log in using an unencrypted session on the same system. In other words,
the PersonaCrypt username is reserved only for PersonaCrypt use. If
necessary to login to both encrypted and unencrypted sessions on the
same system, create two different user accounts; one for each type of
session.

> **note**
>
> Encryption is also possible without requiring removable devices using
> *PEFS*. Refer to the |sysadm| handbook section on
> PEFS Encryption \<pefs\> for more detailed instructions to initialize
> a user with *PEFS*.

Figure %s \<persona1\> shows the PersonaCrypt tab. This is used to
initialize PersonaCrypt for the user.

![User's PersonaCrypt Initialization](images/persona1a.png)

Check Initialize PersonaCrypt Device, insert a removable media device
large enough to hold a user's home directory, then click Select.

> **warning**
>
> Ensure there are no desired files on the removable media. Initializing
> the media for PersonaCrypt formats the device with ZFS and then
> encrypts it with GELI, deleting any existing data.

Input and repeat the Device Password to associate with the device. A
pop-up window indicates the current contents of the device will be
wiped. Click Yes to initialize the device.

To share the computer with other users, create additional login and
*PersonaCrypt* accounts using the |sysadm|
User Manager \<user-manager\>. After creating at least one user, click
Next to continue.

Configure Audio Output
----------------------

Figure %s \<audio1\> shows the Audio Output screen, where you can choose
the output device and test it.

![Configure Audio Output](images/audio1b.png)

Click the Output Device drop-down menu to select the desired sound
device. Click Test to verify the setting. If the device works, a test
sound plays. The Testing Volume slider is also used to set the default
system volume level.

All these settings can be viewed and edited at any time using the
instructions in Sound Mixer Tray.

Connect to a Wireless Network
-----------------------------

> **note**
>
> The network card must be supported by FreeBSD. Refer to
> Supported Hardware for links to FreeBSD support and a list of known
> issues with different hardware.

If the system has an active wireless interface, a screen similar to
Figure %s \<config5\> indicates which wireless networks are
automatically detected. Available networks are ordered by signal
strength.

![Wireless Network Connections](images/config5a.png)

To set the default wireless connection, click the desired network in the
Available Wireless Networks area. If the network requires a password, a
window appears requesting the password and indicating the security type
used by the desired network. If the desired network is not visible in
the Available Wireless Networks area, click Rescan. If unable to connect
or to configure the connection later, refer to Network Manager for more
detailed instructions.

Enable Optional Services
------------------------

Figure %s \<config6\> shows a few optional system services you can
toggle.

![Optional Services](images/config6a.png)

Check Disable IPV6 (Requires Reboot) to reconfigure the system to only
support IPv4 addresses. By default, the system supports both IPv4 and
IPv6, and IPv6 is preferred over IPv4.

> **tip**
>
> Altering this setting does not take affect until the next system
> reboot.

Enable Intel HDA polling enables the audio driver polling mode. It is
used in |trident| to support additional Intel audio devices that would
not function without polling. However, it is recommended to **not**
enable unless you are having extensive audio device issues, or your
Intel device requires polling mode enabled. See the [FreeBSD Manual
Page](https://www.freebsd.org/cgi/man.cgi?query=snd_hda&apropos=0&sektion=4&manpath=FreeBSD+12-current&arch=default&format=html)
for more details.

Enable Realtek Wireless activates the Realtek wireless networking
drivers.

If Enable SSH is checked, the SSH service both starts immediately and is
configured to start on system boot. This option also creates the
firewall rules needed to allow incoming SSH connections to the |trident|
system.

> **danger**
>
> **Do not** check this box if SSH connections to the system are
> undesired.

Enable Verbose Boot is the same option as in boot1. Select this option
to see more detailed messages during the boot process. This can be
useful when troubleshooting a piece of hardware.

When finished choosing optional services, click Next. The screen in
Figure %s \<config7\> indicates the post-installation setup is complete.
Click Finish to access the login menu.

![Setup Complete](images/config7a.png)

Logging In
----------

Once finished setting up the system, the PCDM (|pcbsd| Display Manager)
graphical login screen displays. An example is seen in
Figure %s \<login1\>.

![|trident| Login](images/login1.png)

The hostname of the system is displayed at the top of the login window.
In this example, it is *trident-5026*. This login screen has several
configuration options:

-   **User:** Upon first login, the created **username** (from
    Create a User) is the only available login user. If additional users
    are created using the |sysadm| User Manager \<user-manager\>, they
    are added to the drop-down menu for more login choices. PCDM does
    not allow logging in as the *root* user. Instead, whenever a utility
    requires administrative access, |trident| asks for the password of
    the login account.
-   **Password:** Input the password associated with the selected user.
-   **Desktop:** If any additional desktops are installed using
    AppCafe \<appcafe\>, use the drop-down menu to select the desktop to
    log into.

> **note**
>
> If a PersonaCrypt user is active, insert the PersonaCrypt device in
> order to log in. As seen in Figure %s \<login5\>, this adds an extra
> field to the login screen so the password associated with the
> PersonaCrypt device can be typed.

![|trident| PersonaCrypt Login](images/login5.png)

The toolbar across the bottom of the screen allows several options to be
selected on a per-login basis:

-   **Locale:** If the localization was not set during installation, or
    needs to be changed, click this icon to set the locale for this
    login session.
-   **Keyboard Layout:** Click this icon to change the keyboard layout
    for this login session. This opens the window seen in
    Figure %s \<keyboard1\>.

![Keyboard Settings](images/keyboard1.png)

Click the Keyboard model drop-down menu to select the type of keyboard.

> **note**
>
> The default model of Generic 104-key PC does **not** support special
> keys such as multimedia or Windows keys. Choose another model to
> enable support for hot keys.

This screen also allows selection of the Key Layout and Variant. After
making any selections, test them by typing some text into the
you may type into the space below... field.

> **tip**
>
> It is possible to change keyboard layouts during an active desktop
> session using the included fcitx utility

-   **Restart/Shut Down:** To restart or shutdown the system without
    logging in, click the Power Button icon in the lower-right corner of
    the screen. This icon also allows you to Change DPI, Refresh PCDM,
    and Change Video Driver.

Once any selections are made, input the password associated with the
selected user and press Enter or click the blue arrow to login.
