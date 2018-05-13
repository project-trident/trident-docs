Dual Boot
=========

This chapter discusses the necessary steps to dual boot.

Creating Free Space
-------------------

To dual-boot |Trident| with an existing operating system, first make sure
there is either a free partition or an area of free space to use. For
example, a system running the Windows operating system usually occupies
the entire hard drive. The partition with the current operating system
needs to shrink to make room to install |Trident|. Shrinking is an
operation which retains the current operating system while reducing the
size of its partition. This section demonstrates how to create free
space within Windows 10.

> **warning**
>
> **Before** shrinking a partition, be sure to back up any valuable data
> to an external media such as a removable USB drive!

To shrink the drive, right-click the Start menu and click
Disk Management. In the example shown in Figure %s \<partition1\>, the
Windows system has three partitions: a 450 MB recovery partition, a
237.93 GB data partition, and a 100 MB system partition.

![Disk Layout in Disk Management](images/partition1.png)

This image shows all three Windows partitions filling the entire disk.
The data partition must be shrunk to create space to install |Trident|.
Right-click the data partition (in this example, the *(C:)* partition),
and select Shrink Volume, as shown in Figure %s \<partition2\>.

![Shrink Volume Menu Selection](images/partition2.png)

Wait as the volume is queried for available shrink space. The results
are shown in Figure %s \<shrink1\>.

![Available Shrink Space](images/shrink1.png)

Here, 119307 MB of space is available. This is the maximum amount
Windows can shrink this particular partition. Accept that number, or
choose a smaller number for a smaller |Trident| partition. Click Shrink
to begin the shrinking process. This procedure can take several minutes
to complete. When finished, the newly created free space is displayed as
seen in Figure %s \<shrink2\>.

> **note**
>
> The minimum requirement for a |Trident| install is 20 GB. It is
> recommended to have 50 GB.

![Disk with Free Space](images/shrink2.png)

> **warning**
>
> It is important to **not** choose to install |Trident| into any of the
> three Windows partitions at the Disk Selection screen of the
> installer. It is a good idea to write down the sizes of all of the
> partitions so the free space is recognizable when the |Trident|
> installer displays the current partitions.

Requirements for Dual Booting
-----------------------------

Dual booting with |Trident| has several requirements:

-   An *EFI* or *UEFI* partitioning scheme. |Trident| does not support
    the older MBR partition scheme, opting instead to use
    [rEFInd](http://www.rodsbooks.com/refind/) for managing or booting
    into operating systems. Trident still uses the BSD boot loader, as it
    provides native support for ZFS boot environments. Be sure to select
    Install rEFInd when installing |Trident| (see install7).
-   A partition for each operating system. Many operating systems,
    including |Trident|, can only be installed into a primary or *GPT*
    partition. See Creating Free Space for an example of shrinking a
    disk in Windows to allow for dual booting with |Trident|.
-   Back up any existing data! It is recommended to store this backup on
    a different computer, removable media such as a USB drive or DVD
    media.

Dual Booting
------------

A |Trident| installation assumes there is an existing *GPT* or primary
partition for installation. If the computer has only one disk and
|Trident| is the only operating system, it is fine to accept the default
partitioning scheme. However, if |Trident| is to share space with other
operating systems, ensure |Trident| is installed into the correct
partition, or an existing operating system may be overwritten.

> **note**
>
> As adjusting the partitions/spacing on active disks can be a
> complicated and difficult process, it is recommended to partition your
> disk for dual booting before installing any operating systems.

When installing |Trident| onto a computer meant to contain multiple
operating systems, carefully select the **correct** partition in the
Disk Selection screen. On a system containing multiple partitions, each
partition is listed.

> **danger**
>
> Avoid selecting a partition containing an operating system or
> essential data.

Highlight the desired partition and click Customize. Clicking Next
without customizing the disk layout results in the installer overwriting
the contents of the primary disk.

Once installed, the system boots into the rEFInd menu seen in
Figure %s \<refind1\>.

![rEFInd Boot Manager](images/refind1.png)

rEFInd displays any installed operating systems and boots into the
default choice after a few seconds. Press any key other than Enter to
pause automatic booting, then use the arrow keys to select the desired
operating system. Press Enter to continue booting.
