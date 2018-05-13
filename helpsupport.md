Help and Support
================

The |trident| Project strives to make using |trident| as easy as possible
for newcomers. If help is needed, there are many ways to get in touch
with the |trident| community. This chapter describes the available
resources for troubleshooting |trident|.

As a teacher may have said, "there is no such thing as a stupid
question". However, there are ways to ensure a productive exchange for
all parties involved. The two articles below describe how and why it is
important to follow certain protocols when requesting help:

-   [How to Ask Smart Questions
    (PDF)](http://divajutta.com/doctormo/foo/ask-smart-questions.pdf)
-   [How To Ask Questions The Smart
    Way](http://catb.org/~esr/faqs/smart-questions.html)

Troubleshooting
---------------

This section contains instructions to troubleshoot user discovered
issues with |trident|. These instructions are not exhaustive, but can
serve as a starting point when encountering problems with |trident|. If
the particular issue is missing from this section, see the section about
the trident Community for instructions about asking for help from the
wider community.

### Display

If problems exist with the display settings and manually editing
/etc/X11/xorg.conf or running Xorg --config is necessary, first tell the
|trident| system to not automatically start X. To do this, add
pcdm\_enable="NO" temporarily to /etc/rc.conf, then reboot the system.

The system reboots to a login prompt. After logging in, follow the
instructions in the FreeBSD
[Handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/x-config.html)
to manually configure and test Xorg. Once a working configuration is
found, save it to /etc/X11/xorg.conf. Then, remove the temporary line
shown above from /etc/rc.conf and start PCDM with service pcdm start.

If the graphics white-out after a suspend or resume, run
sysctl hw.acpi.reset\_video=1 as the superuser.

If the problem is fixed, carefully add hw.acpi.reset\_video=1 to
/etc/sysctl.conf.

If the monitor goes blank and does not come back, run xset -dpms as the
regular user account.

If the problem is fixed, add xset -dpms to the .xprofile file in the
user's home directory.

If any display settings change, click Apply for the settings to be
tested. If anything goes wrong during testing, the system returns to the
Display Settings screen for the user to try another setting. Once
satisfied with the tested setting, click "Yes to save the setting and
proceed. Alternately, click Skip to configure the display settings
later.

### Installation

Installing |trident| is usually very simple. However, sometimes problems
occur. This section examines solutions to the most common installation
problems.

The |trident| installer creates a log which keeps a record of all the
completed steps, as well as any errors. When an installation error
occurs, the |trident| installer asks to generate an error report. If Yes
is chosen, a pop-up message asks to save the error log to a USB stick.
Type y and insert a FAT formatted USB thumb drive to copy the log.

While in the installer, read this log to see what went wrong. Click the
black Emergency Shell and Utilities icon, then select shell from the
|trident| Utility Menu. Read the log by typing more /tmp/.SysInstall.log.

If the error can not be fixed or you believe an installation bug exists,
include the log saved on the USB stick in your bug report by following
the instructions in Report a Bug.

If the installer does not arrive at the initial GUI installer screen,
try unplugging as many devices as possible, such as webcams, scanners,
printers, USB mice and keyboards. If this solves the problem, plug in
one piece of hardware at a time, then reboot. This helps pinpoint which
device is causing the problem.

Additionally, you may need to enable **EFI** in Virtualbox by navigating
Settings --\> System --\> Motherboard and checking
Enable EFI (special OSes only).

If the computer freezes while probing hardware and unplugging extra
devices does not fix the problem, it is possible that the installation
media is corrupt. If the Data Integrity check on the downloaded file is
correct, try burning the file again at a lower speed.

If the system freezes and the video card is suspected to be the cause,
review the system's BIOS settings. If there is a setting for video
memory, set it to its highest value. Also, check to see if the BIOS is
set to prefer built-in graphics or a non-existent graphics card. On some
systems this is determined by the order of the devices listed; in this
case, be sure the preferred device is listed first. If the BIOS settings
are invisible, move a jumper or remove a battery to make it revert to
the default built-in graphics; check the manual or contact the card
manufacturer for details.

A common cause for problems is the *LBA* (Logical Block Addressing)
setting in the BIOS. If the PC is not booting before or after
installation, check the BIOS and turn *LBA* off (do not leave it on
automatic).

If the SATA settings in the BIOS are set to *compatibility* mode, try
changing this setting to *AHCI*. If the system hangs with a BTX error,
try turning off *AHCI* in the BIOS.

If the USB keyboard is non-functional, check if there is an option in
the BIOS for *legacy support* in relation to the keyboard, USB, or both.
Enabling this feature in the BIOS may solve this issue.

If the installer boots and a *mountroot\>* command prompt appears, this
may be due to a change in the location of the boot device. This can
occur when the enumeration of a card reader changes. The solution is to
enter ufs:/dev/da1 at the prompt. Depending on the exact location of the
boot media, it may be different from da1. Type ? at the prompt to
display the available devices.

If none of the above has fixed the problem, the trident Community is a
valuable resource to assist in tracking down and solving the issue.

### Network

While networking usually "just works" on a |trident| system, users
sometimes encounter problems, especially when connecting to wireless
networks. Sometimes the problem is due to a configuration error or
sometimes a driver is buggy or unavailable. This section is meant to
help pinpoint the problem, so you can either personally fix it or give
the developers the information they need to fix or create a driver.

When troubleshooting the network configuration, use these files and
commands.

The /etc/rc.conf file is read when the system boots up. In order for the
system to configure an interface at boot time, an entry must exist for
it in this file. Entries are automatically created during installation
for each active interface. An entry is added (if it does not exist) or
modified (if it already exists) when configuring an interface using the
Network Manager.

Here is an example of the rc.conf entries for an ethernet driver
(**em0**) and a wireless driver (**run0**):

``` {.sourceCode .none}
ifconfig_em0="DHCP"
wlans_iwm0="wlan0"
ifconfig_wlan0="WPA SYNCDHCP"
```

When reading your own file, look for lines beginning with **ifconfig**.
For a wireless interface, also look for lines containing **wlans**.

> **note**
>
> Unlike Linux interface driver names, FreeBSD/|trident| interface driver
> names indicate the type of chipset. Each driver name has an associated
> manual page where you can learn which devices use that chipset and if
> there are any configuration options or limitations for the driver.
> When reading the man page, do not include the interface number. For
> the above example, read man em and man iwm.

/etc/wpa\_supplicant.conf is used by wireless interfaces and contains
the information needed to connect to a WPA network. If this file does
not already exist, it is created when entering the Configuration screen
of a wireless interface.

The command ifconfig shows the current state of the interfaces. When
reading through its output, ensure the desired interface is listed, has
a status of **active**, and has an IP address. Here is a sample ifconfig
output showing the entries for an *re0* Ethernet interface and a *run0*
wireless interface:

``` {.sourceCode .none}
re0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500 options=389b<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,VLAN_HWCSUM,WOL_UCAST,WOL_MCAST,WOL_MAGIC>
ether 60:eb:69:0b:dd:4d
inet 192.168.1.3 netmask 0xffffff00 broadcast 192.168.1.255
media: Ethernet autoselect (100baseTX <full-duplex>)
status: active

run0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 2290
ether 00:25:9c:9f:a2:30
media: IEEE 802.11 Wireless Ethernet autoselect mode 11g
status: associated

wlan0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
ether 00:25:9c:9f:a2:30
media: IEEE 802.11 Wireless Ethernet autoselect (autoselect)
status: no carrier
ssid "" channel 10 (2457 MHz 11g)
country US authmode WPA1+WPA2/802.11i privacy ON deftxkey UNDEF
txpower 0 bmiss 7 scanvalid 60 protmode CTS wme roaming MANUAL bintval 0
```

In this example, the ethernet interface (*re0*) is active and has an IP
address. However, the wireless interface (*run0*, which is associated
with *wlan0*) has a status of **no carrier** and does not have an IP
address. In other words, it has not yet successfully connected to the
wireless network.

The dmesg command lists the hardware probed during boot time and
indicates if the associated driver was loaded. To search the output of
this command for specific information, pipe it to grep as seen in this
example:

``` {.sourceCode .none}
dmesg | grep Ethernet
re0: <RealTek 8168/8111 B/C/CP/D/DP/E PCIe Gigabit Ethernet> port 0xc000-0xc0ff mem 0xd0204000-0xd0204fff,0xd0200000-0xd0203fff irq 17 at device 0.0 on pci8
re0: Ethernet address: 60:eb:69:0b:dd:4d

dmesg |grep re0
re0: <RealTek 8168/8111 B/C/CP/D/DP/E PCIe Gigabit Ethernet> port 0xc000-0xc0ff mem 0xd0204000-0xd0204fff,0xd0200000-0xd0203fff irq 17 at device 0.0 on pci8
re0: Using 1 MSI messages
re0: Chip rev. 0x28000000
re0: MAC rev. 0x00000000 miibus0: <MII bus> on re0
re0: Ethernet address: 60:eb:69:0b:dd:4d
re0: [FILTER]
re0: link state changed to DOWN
re0: link state changed to UP

dmesg | grep run0
run0: <1.0> on usbus3
run0: MAC/BBP RT3070 (rev 0x0201), RF RT2020 (MIMO 1T1R), address 00:25:9c:9f:a2:30
run0: firmware RT2870 loaded
```

If the desired interface does not show up with ifconfig or dmesg, it is
possible a driver for this card is not provided with the operating
system. If the interface is built into the motherboard of the computer,
use the pciconf command to discover the type of card:

``` {.sourceCode .none}
pciconf -lv | grep Ethernet
device = 'Gigabit Ethernet NIC(NDIS 6.0) (RTL8168/8111/8111c)'

pciconf -lv | grep wireless
device = 'Realtek RTL8191SE wireless LAN 802.11N PCI-E NIC (RTL8191SE?)'
```

In this example, there is a built-in Ethernet device using a driver
which supports the *RTL8168/8111/8111c* chipsets. As we saw earlier, the
driver is *re0*. The built-in wireless device was also found but the *?*
indicates a driver for the *RTL8191SE* chipset was not found. A web
search for **FreeBSD RTL8191SE** gives an indication if a driver exists
or is being developed.

The FreeBSD Handbook chapter on [Wireless
Networking](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/network-wireless.html)
provides a good overview of how wireless works and offers additional
troubleshooting suggestions.

### Printer

Here are some solutions to common printing problems:

-   **A test page prints but it is all garbled:** This typically means
    the system is using the wrong driver. If your specific model was not
    listed, click Adminstration --\> Modify Printer for the printer in
    the Printers tab. In the screen shown in print7, try choosing
    another driver close to your model number. If trial and error does
    not fix the problem, see if there are any suggestions for your model
    in the [Open Printing
    database](http://www.openprinting.org/printers). A web search for
    *freebsd* followed by the printer model name may also help you find
    the correct driver to use.
-   **Nothing happens when you try to print:** In this case, type
    tail -f /var/log/cups/error\_log in a console and then try to print
    a test page. Any error messages will appear in the console. If the
    solution is not obvious from the error messages, try a web search
    for the error message. If you are still stuck, post the error, the
    model of your printer, and your version of |trident| as you
    Report a Bug.

### Replication

This is a recreation of the user submitted article: **Forcibly resetting
ZFS replication using the command line** **lpreserver**. A special
**"thank you!"** to |trident| user **VulcanRidr** for providing this
article.

ZFS \<ZFS Overview\> replication can be somewhat complex, and keeping
all the fiddly bits aligned can be fraught with danger.

I recently had both of my |trident| machines start failing to replicate.
My desktop is called **defiant** and it has two pools: **NX74205** and
**NCC1764**. My laptop is named **yukon** with a single pool,
**NCC74602**. I am replicating to my FreeNAS server, named **luna**, to
the dataset NX80101/archive/\<FQDN\>. I will focus on what I did to get
**yukon** working again in this document.

#### Original Indications

The |sysadm| client tray icon was pulsing red. Right-clicking on the
icon and clicking Messages showed the message:

``` {.sourceCode .none}
FAILED replication task on NCC74602 -> 192.168.47.20: LOGFILE: /var/log/lpreserver/lpreserver_failed.log
```

This was lifted from /var/log/lpreserver/lpreserver.log.
/var/log/lpreserver/lastrep-send.log shows very little information:

``` {.sourceCode .none}
send from @auto-2017-07-12-01-00-00 to NCC74602/ROOT/12.0-CURRENT-up-20170623_120331@auto-2017-07-14-01-00-00
total estimated size is 0
TIME        SENT    SNAPSHOT
```

And no useful errors were being written to the lpreserver\_failed.log.

#### Repairing Replication

**First Attempt:**

My first approach was to use the |sysadm| Client (see the
Life Preserver \<life-preserver\> section for more details).

Figure %s \<helprep1\> shows my Life Preserver Replication tab:

> Attempt 1: GUI Replication Repair

I clicked on the dataset in question, then clicked Initialize. I waited
for a few minutes, then clicked Start. I was immediately rewarded with a
pulsing red icon in the system tray and received the same messages as
noted above.

**Second Attempt:**

I was working with and want to give special thanks to users
<*@RodMyers>\* and <*@NorwegianRockCat*>. They suggested I use the
lpreserver command line. So I issued these commands:

``` {.sourceCode .none}
sudo lpreserver replicate init NCC74602 192.168.47.20
sudo lpreserver replicate run NCC74602 192.168.47.20
```

Unfortunately, the replication failed again, with these messages:

``` {.sourceCode .none}
Fri Jul 14 09:03:34 EDT 2017: Removing NX80101/archive/yukon.sonsofthunder.nanobit.org/ROOT - re-created locally
cannot unmount '/mnt/NX80101/archive/yukon.sonsofthunder.nanobit.org/ROOT': Operation not permitted
Failed creating remote dataset!
cannot create 'NX80101/archive/yukon.sonsofthunder.nanobit.org/ROOT': dataset already exists
```

It turned out there were a number of child sets. I logged into the
FreeNAS (**luna**) and issued this command as **root**:

\# zfs destroy -r NX80101/archive/defiant.sonsofthunder.nanobit.org

Then I ran the replicate init and replicate run commands again from the
|trident| host. Replication now works and continues to work, at least
until the next fiddly bit breaks.

### Sound

Type mixer from the command line to see the current sound settings

``` {.sourceCode .none}
mixer
Mixer vol      is currently set to   0:0
Mixer pcm      is currently set to 100:100
Mixer speaker  is currently set to 100:100
Mixer mic      is currently set to  50:50
Mixer rec      is currently set to   1:1
Mixer monitor  is currently set to  42:42
Recording source: monitor
```

If any of these settings are set to *0*, set them to a higher value by
specifying the name of the mixer setting and a percentage value up to
*100*:

``` {.sourceCode .none}
mixer vol 100
Setting the mixer vol from 0:0 to 100:100.
```

To make the change permanent, create a file named .xprofile in the home
directory containing the corrected mixer setting.

If only one or two mixer settings are available, the default mixer
channel needs to change. As the superuser, use
sysctl -w hw.snd.default\_unit=1 to alter the mixer channel.

To see if the mixer has changed to the correct channel, type mixer
again. If there are still only one or two mixer settings, try setting
the sysctl value to *2*, and, if necessary, *3*.

Once all of the mixer settings appear and none are set to *0*, sound
typically works. If it still does not, these resources can help pinpoint
the problem:

-   [FreeBSD Handbook Sound
    Section](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/sound-setup.html)
-   [FreeBSD Sound Wiki](https://wiki.FreeBSD.org/Sound)

If sound issues persist, consider asking the trident Community for help
or Report a Bug. When reporting an issue, be sure to include both the
version of |trident| and name of the sound card.

Upgrading from |pcbsd| 10.x to |trident|
---------------------------------------

> **warning**
>
> If any user account uses PersonaCrypt, please be sure to save any
> encryption keys to a safe place (e.g. a thumb drive) before beginning
> the upgrade process. Loss of encryption keys may result in being
> unable to import the home directory after the upgrade is complete.

If the system is using |pcbsd| 10.x, the option to update to |trident|
does not appear in the Control Panel version of Update Manager. This is
because a new installation is required in order to migrate to |trident|.
However, the |trident| installer allows the user to keep all their
existing data and home directories as it provides the ability to install
|trident| into a new boot environment. In other words, the new operating
system and updated applications are installed while the ZFS pool and any
existing boot environments are preserved. Since the new install is in a
boot environment, the option to boot back into the previous |pcbsd|
installation remains.

> **note**
>
> This option overwrites the contents of /etc. If any custom
> configurations exist, save them to a backup or the home directory
> first. Alternately, use the |sysadm|
> Boot Environment Manager \<boot-environment-manager\>
> post-installation to mount the previous |pcbsd| boot environment to
> copy over any configuration files which may not have been backed up.

To perform the installation to a new boot environment, start the
|trident| installation as described earlier in the chapter. In the
System Selection screen, choose to install either a desktop or a server.
Press Next to view the Disk Selection screen, shown in
Figure %s \<upgrade1\>.

![Disk Selection](images/upgrade1b.png)

|trident| automatically detects if the drive has an existing boot
environment and fills in the data as necessary. If no boot environments
are detected, Install into Boot Environment is invisible. To upgrade,
select Install into Boot Environment and choose which existing pool to
install into from the drop-down menu. In the
Disk Selection Screen \<upgrade1\>, the user is installing into the
existing **tank** pool. Press Next when ready.

> **warning**
>
> Be sure Install into Boot Environment is checked before proceeding, or
> data can be lost.

A pop-up will appear and ask to start the default Full-Disk
installation. Click Yes to begin the installation.

When the installation is complete, reboot the system and remove the
installation media. The post-installation screens run as described in
the Booting Into trident \<Booting Into trident\> section to help
configure the new installation.

> **warning**
>
> During the Create a User process, recreate the primary user account
> using the same user name and user id (UID) from the previous |pcbsd|
> system. This allows |trident| to associate the existing home directory
> with that user. Once logged in, use the |sysadm|
> User Manager \<user-manager\> to recreate any other user accounts or
> to reassociate any PersonaCrypt accounts.

The |trident| Community
----------------------

The |trident| community has grown and evolved since the project's
inception. A wide variety of chat channels and forum options are now
available for users to interact with each other, contributors to the
project, and the core development team.

### Telegram Community

The |trident| Project uses [Telegram](https://telegram.org/) to provide
real-time chat and collaboration with |trident| users and developers.
Telegram is available in a web browser or as downloadable applications
for Android, macOS, and PC/Mac/Linux.

To access the trident Telegram community, point a web browser to
<https://web.telegram.org>. Sign in with a valid phone number and join
the [trident Community
channel](https://web.telegram.org/#/im?p=@tridentCommunity).

Telegram maintains a full archive of the chat history. This means
lengthy conversations about hardware issues or workarounds are always
available for reference.

Here are some tips about the community channel:

-   Most of the regular users are always logged in, even when they are
    away from their computer or are busy doing other things. If no one
    responds immediately, do not get mad, leave the channel, and never
    come back again. Stick around for a while to see if anyone responds.
-   Users represent many different time zones. It is quite possible it
    is late at night or very early in the morning for some users when
    asking a question.
-   Do not post large error messages in the channel. Instead, use a
    pasting service such as <https://pastebin.com/> and refer to the URL
    on channel.
-   Be polite and do not demand a response from others.
-   It is considered rude to "Chat Privately" with someone who does not
    know you without first asking their permission. If no one answers
    the question, do not start chatting privately with unkown people in
    the room.
-   The first time joining the channel, it is okay to say "hi" and
    introduce yourself. If a new person joins the channel, feel free to
    welcome them and to make them feel welcome.

### |trident| Subreddit

The |trident| Project also has a
[Subreddit](https://www.reddit.com/r/trident/) for users who prefer to
use Reddit to ask questions and to search for or post how-tos. A Reddit
account is not required in order to read the Subreddit, but it is
necessary when submitting new posts or commenting on existing posts.

### Discourse

|trident| also has a [Discourse forum](https://discourse.trident.org/)
managed concurrently with the Subreddit. Functionally similar to the
Subreddit, a new user needs to sign up with Discourse in order to create
posts, but it is possible to view the current posts without an account.

### IRC

Like many open source projects, |trident| has an Internet Relay Chat
(IRC) channel so users can chat and get help in real time. To get
connected, use this information in your IRC client:

-   Server name: irc.freenode.net
-   Channel name: \#trident (note the \# is required)

|appcafe| has an IRC category where you can find IRC client software. If
you do not wish to install an IRC client, you can use the web interface
to view \#trident: <https://webchat.freenode.net/>

IRC is a great way to chat with other users and get answers to your
questions. Here are a few things to keep in mind if you ask a question
on IRC:

-   Most of the regular users are always logged in, even when they are
    away from their computer or are busy doing other things. If you do
    not get an answer right away, do not get mad, leave the channel, and
    never come back again. Stick around for a while to see if anyone
    responds.
-   IRC users represent many different time zones. It possibly late at
    night or very early in the morning for some users when you ask a
    question.
-   Do not post error messages in the channel as the IRC software can
    kick you out for flooding and it is considered to be bad etiquette.
    Instead, use a pasting service such as
    [pastebin](https://pastebin.com/) and refer to the URL on channel.
-   Be polite and do not demand that others answer your question.
-   It is considered rude to DM (direct message) someone who does not
    know you. If no one answers your question, do not start DMing people
    you do not know.
-   The first time you join a channel, it is okay to say "hi" and
    introduce yourself.

### Social Media

The |trident| project maintains several social media sites to help users
keep up-to-date with what is happening and to provide venues for
developers and users to network with each other. Anyone is welcome to
join.

-   [Official trident Blog](https://www.trident.org/blog/)
-   [trident Project on Twitter](https://twitter.com/trident_Project/)
-   [trident Facebook
    Group](https://www.facebook.com/groups/4210443834/)
-   [trident LinkedIn
    Group](https://www.linkedin.com/start/join?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fgroups%2F1942544&trk=login_reg_redirect)

Contributing to |trident|
------------------------

Many in the |trident| community have assisted in its development,
providing valuable contributions to the project. |trident| is a large
project with many facets, meaning there is ample opportunity for a wide
variety of skill sets to easily improve the project.

### Report a bug

One of the most effective ways to assist the |trident| Project is by
reporting problems or bugs encountered while using |trident|. Anyone can
report a |trident| bug. Here is a rundown of the |trident| bug reporting
tools:

-   |trident| uses a [GitHub repository](https://github.com/trident/) to
    manage bugs. A GitHub account is required before bugs can be
    reported. Navigate to <https://github.com/>, fill in the required
    fields, and click Sign up for GitHub to create a new GitHub account.

> **note**
>
> The GitHub issues tracker uses email to update contributors on the
> status of bugs. Please use a valid and frequently used email address
> when creating a GitHub account.

-   The |trident| code is organized into many repositories representing
    the |lumina| desktop, the graphical utilities, |sysadm|, and various
    other applications. When reporting a bug, select the
    [trident-core](https://github.com/trident/trident-core) repository. If
    the bug is specific to |lumina|, use the
    [lumina](https://github.com/trident/lumina) repository. Documentation
    bugs are tracked in their respective *-docs* repositories. Issues
    with any project website are tracked in
    [trident-website](https://github.com/trident/trident-website).
-   After clicking a repository's Issues tab, use the *search* bar to
    confirm no similar bug report exists. If a similar report does
    exist, add any additional information to the report using a comment.
    While it is not required to log in to search existing bugs, adding a
    comment or creating a new report does require signing into GitHub.
-   To create a new bug report, navigate to the desired repository and
    click Issues --\> New Issue. Figure %s \<bug1\> shows the creation
    of a new bug report.

![Creating a Bug Report](images/bug1.png)

Here are some basic guidelines for creating useful bug reports:

**Title Area**

The ideal title is clear, concise, and informative. Here are some
recommendations for creating a title:

-   Be objective and clear (and refrain from using idioms or slang).
-   Include the application name if the issue is related to an
    application.
-   Include keywords from any error messages you receive.
-   Avoid using vague language such as "failed", "useless", or
    "crashed".

Here are some examples to show the difference between a helpful title
and a non-helpful title:

``` {.sourceCode .none}
Example 1:

Non-Helpful:
Lumina-FM crashed.
Helpful:
Lumina-FM crashed after clicking on a directory name.

Example 2:

Non-Helpful:
Extracting an archive doesn't work.
Helpful:
Lumina-Archiver shows the error "file not supported" when opening a
.cab file.
```

**Comment Area**

Like with the *title*, being clear and concise is extremely helpful.
Many people feel they must fill this area with lots of information.
While listing a lot of information seems helpful, specific details are
often more useful in issue resolution.

The most important pieces of information to include are:

A)  What happened.
B)  What you expected to happen.
C)  (**Critical**) Steps to reproduce the issue. Please provide the
    exact steps you can take to produce this issue from a fresh boot. If
    the issue is application specific, provide the exact steps from a
    fresh start of the application.
D)  List any changes you may have made to your system from its initial
    install. In most cases, this does not need to be extremely detailed.
    It is very helpful for contributors to know if you have installed or
    removed any major applications or if you have changed any OS
    settings. If you are unsure of all your changes, list what comes to
    mind.
E)  List the hardware of the system where the issue occurred. If you are
    using an OEM laptop or desktop, listing the brand or model is
    usually sufficient. If the issue is wireless related, please check
    the system manufacturer's website for your brand or model and let us
    know what wireless cards may be shipped in your laptop. If you are
    using a custom built desktop, all we primarily need to know is CPU,
    RAM, and GPU. If you happen to know the motherboard model, please
    include it too. Attaching a copy of /var/run/dmesg.boot is also
    helpful, as this file shows the hardware probed the last time the
    |trident| system booted. Finally, including the output of uname -a is
    helpful.

Being clear and direct with your report and answers is very helpful. As
we are not watching you use your computer and do not see what you see,
we are totally dependent on your explanation. We only know what you tell
us. Some users worry they have not provided enough information when they
file a ticket. In most cases, providing the information for these five
items is sufficient. If more information is required, you may see
questions posted to your bug report.

**Additional Information**

Please do not think you are unable to file your bug ticket without
additional information. Providing the listed information above is the
most important information for contributors to know. Providing logs does
not help as much as those five pieces of information. In some cases,
only providing logs to an otherwise empty bug report results in our
being unable to resolve your issue.

Additionally useful information may include:

-   Screen captures of the error.
    Lumina Screenshot \<luminautl.html\#screenshot\> is a useful tool to
    quickly screenshot any errors in progress.
-   Command Line Output Logs
-   Truss Logs
-   Debugger Backtrace Logs

After describing the issue, click Submit new issue to create the issue.
The bug tracker attaches a unique number to the report and sends update
messages to your registered email address whenever activity occurs with
the bug report.

### Become a Beta Tester

If you enjoy tinkering with operating systems and have a bit of spare
time, one of the most effective ways to assist the |trident| community is
reporting problems you encounter while using |trident|.

If a spare system or virtual machine is available, you can also download
and test the latest UNSTABLE patches (see Updating trident). Having as
many people as possible using |trident| on many different hardware
configurations assists the Project in finding and fixing bugs. The end
result is more polished and usable OS for the entire community.

If you wish to become a tester, join the Telegram [trident Community
channel](https://web.telegram.org/#/im?p=@tridentCommunity). Updates are
typically announced announced here. You can also see any problems other
testers are finding and can check to see if the problem exists on your
hardware as well.

Anyone can become a beta tester. If you find a bug while testing, be
sure to accurately describe the situation when
Reporting a bug \<Report a bug\> so it can be fixed as soon as possible.

### Translation

If interested in translating |trident| into your native language, start
by choosing which of the three translation areas to work in:

1.  Translate the graphical menus within the |trident| operating system.
2.  Translate the documentation published with |trident|.
3.  Translate the |trident| website.

This section describes each of these translation areas in more detail
and how to begin as a translator.

Regardless of the type of desired translation, you should first join the
[trident Community
channel](https://web.telegram.org/#/im?p=@tridentCommunity). The first
time joining the channel, introduce yourself and indicate which
language(s) and which type(s) of translations you can assist with. This
allows you to meet other volunteers and stay informed of any notices or
updates affecting translators.

#### Interface Translation

|trident| uses [Weblate](https://weblate.org/en/) for managing
localization of the menu screens used by the installer and the |trident|
utilities. Weblate makes it easy to find out if your native language has
been fully localized for |trident|. It also makes it easy to verify and
submit translated text, as it provides a web editor and commenting
system. This means translators can spend more time making and reviewing
translations rather than learning how to use a translation tool.

To assist with a localization, open the [trident translation
projects](https://weblate.trident.org/projects/) in a web browser. An
example is seen in Figure %s \<translate1\>.

![|trident| Weblate Translation System](images/translate1a.png)

Before editing a translation, first create a a login account and verify
the activation email. Once logged in, click Manage your languages, shown
in Figure %s \<translate2\>.

![Weblate Dashboard](images/translate2.png)

In the screen shown in Figure %s \<translate3\>, use the
Interface Language drop-down menu to select the language for the Weblate
interface. Then, in Translated languages, use the arrows to add or
remove the languages you wish to translate. Once any selections are
made, click Save.

![Manage Languages](images/translate3.png)

> **note**
>
> If the language you wish to translate is missing from the "Translated
> languages" menu, request its addition in the [trident Community
> channel](https://web.telegram.org/#/im?p=@tridentCommunity).

Next, click Projects at the top of the screen to select a localization
project. In the example shown in Figure %s \<translate4\>, the user has
selected the *trident-utils-qt5* project, which represents the
localization of the |trident| graphical interface. This screen shows the
components of the project and the current progress of each component's
translation. The green bar indicates the localization percentage. If a
component is not at 100%, this means its untranslated menus will instead
appear in English.

![Project Selection](images/translate4.png)

To start translating, click a component name. In the screen shown in
Figure %s \<translate5\>, select a language and click Translate.

![Translation Languages](images/translate5.png)

In the example shown in Figure %s \<translate6\>, the user has selected
to translate the *pc-installgui* component into the Spanish language.
The English text is displayed in the Source field and the translator can
type the Spanish translation into the Translation field. Use the arrows
near the Strings needing action field to navigate between strings to
translate.

![Translation Editor](images/translate6.png)

If assistance is needed with either a translation or the Weblate system,
ask for help in the [trident Community
channel](https://web.telegram.org/#/im?p=@tridentCommunity).

#### Documentation Translation

The source for the |trident| Users Handbook is stored in the [trident
github
repository](https://github.com/trident/trident-docs/tree/master/trident-handbook).
This allows the documentation and its translations to be built with the
operating system. Documentation updates are automatically pushed to the
|trident| website and, when the system is updated using the |sysadm|
Update Manager \<update-manager\>, the doc updates are installed to a
local copy named /usr/local/share/trident/handbook/trident.html. This
keeps an updated local copy of the handbook available on every user's
system.

The |trident| build server provides the HTML version of the |trident|
Users Handbook. Instructions for building your own HTML, PDF, or EPUB
version can be found in this
[README.md](https://github.com/trident/trident-docs/blob/master/trident-handbook/README.md).

The documentation source files are integrated into the Weblate
translation system so the |trident| documentation can be translated using
a web browser. The process is similar to Interface Translation except
**trident-guide** must be selected from the Projects drop-down menu shown
in translate4.

It is important to be aware of a few elements when translating the
documentation:

At this time, some formatting tags are still displayed in raw text, as
seen in the examples in Figure %s \<translate7\> and
Figure %s \<translate8\>.

> **danger**
>
> Do not remove the formatting as this can break the documentation build
> for that language.

In translate7, it is fine to translate the phrase "Using the Text
Installer", but care must be taken to avoid removing any of the
surrounding colons and backticks, or to change the text of the *ref*
tag. In translate8, the asterisks are used to bold the words *bare
minimum*. It is fine to translate *bare minimum*, but do **not** remove
the asterisks.

![Formatting Characters - Do Not Remove](images/translate7.png)

![More Formatting Characters](images/translate8.png)

To build a local HTML copy that includes the latest translations, either
for personal use or to visualize the translated Guide, type these
commands from the command line in |trident|:

``` {.sourceCode .none}
sudo pkg install trident-toolchain
rehash
git clone git://github.com/trident/trident-docs
cd trident-docs/trident-handbook
sudo make i18n
make html
ls _build
doctrees                html-es                 html-tr    trident-handbook-i18n.txz
html                    html-fr                 html-uk
html-da                 html-id                 locale
html-de                 html-pt_BR              locale-po
```

This makes an HTML version of the Guide for each of the available
translations. In this example, translations are available for English
(in html), Danish, German, Spanish, French, Indonesian, Brazilian
Portuguese, Turkish, and UK English. To update the HTML at a later time:

``` {.sourceCode .none}
cd ~/trident-docs
git pull
cd trident-docs/trident-handbook
sudo make i18n
sudo make html
```

#### Website Translation

If you are interested in translating the |trident| website, introduce
yourself in the [trident Community
channel](https://web.telegram.org/#/im?p=@tridentCommunity) or open a new
topic in our [Discourse forum](https://discourse.trident.org/)

Currently, the website is being translated into several languages,
including: Dutch, French, German, Polish, Spanish, Swedish, and Turkish.

### Development

If you like programming, and especially coding on FreeBSD, we would love
to see you join the |trident| team as a |trident| contributor. Developers
who want to help improve the |trident| codebase are always welcome! To
participate in core development, introduce yourself in the [trident
Discourse forum](https://discourse.trident.org/). Feel free to browse the
Issues in the [trident repository](https://github.com/trident/). If you
see something you want to work on, or have a proposal for a project to
add to |trident|, mention it and someone will be happy to help you get
started.

Most of the |trident| specific GUI tools are developed in C++ using Qt
libraries and other non-GUI development is done using standard Bourne
shell scripts. There may be cases where other languages or libraries are
needed, but those are evaluated on a case-by-case basis.

#### Getting the Source Code

The |trident| source code is available from
[GitHub](https://github.com/trident/). The code is organized into
repositories which represent the |lumina| desktop, the graphical
utilities, |sysadm|, and various other applications. git needs to be
installed in order to download the source code. When using |trident|, git
is included in the base install.

To download the source code, cd to the directory to store the source
code and specify the name of the desired repository. In this example,
the user is downloading the source for the graphical utilities:

``` {.sourceCode .none}
~% cd Projects
~/Projects% git clone git://github.com/trident/trident-utils-qt5
```

This creates a directory with the same name as the repository.

> **note**
>
> To keep the local copy in sync with the official repository,
> periodically run git pull within the directory.

Before compiling any source, ensure the Ports Collection is installed.
At this time, **git** is used to fetch and update ports (see
FreeBSD Ports).

Fetching ports for the first time (as root):

``` {.sourceCode .none}
# git clone http://github.com/trident/freebsd-ports.git /usr/ports
```

Update an existing ports directory (as root):

``` {.sourceCode .none}
# cd /usr/ports

# git pull
```

Then, cd to the directory containing the source to build and run the
mkport.sh script. In this example, the developer wants to compile the
graphical utilities:

``` {.sourceCode .none}
cd trident-utils-qt5

./mkport.sh /usr/ports/
```

This creates a port which can then be installed. The name of the port is
located in mkport.sh. This example determines the name of the port
directory, changes to it, and then builds the port. Since this system is
already running the |trident| graphical utilities, reinstall is used to
overwrite the current utilities:

``` {.sourceCode .none}
grep port= mkport.sh
port="sysutils/trident-utils-qt5"
cd /usr/ports/sysutils/trident-utils-qt5
make reinstall
```

If you plan to make source changes, several Qt IDEs are available in the
|sysadm| AppCafe \<appcafe\>. The
[QtCreator](http://wiki.qt.io/Category:Tools::QtCreator) application is
a full-featured IDE designed to help new Qt users get up and running
faster while boosting the productivity of experienced Qt developers. [Qt
Designer](http://doc.qt.io/archives/qt-4.8/designer-manual.html) is
lighter weight as it is only a .ui file editor and does not provide any
other IDE functionality.

If planning to submit changes for inclusion in |trident|, fork the
repository using the instructions in [fork a
repo](https://help.github.com/articles/fork-a-repo). Make your changes
to the fork, then submit them by issuing a [git pull
request](https://help.github.com/articles/using-pull-requests). Once
your changes have been reviewed, they can either be committed or
returned with suggestions for improvement.

#### Design Guidelines

|trident| is a community driven project relying on the support of
developers in the community to help in the design and implementation of
new utilities and tools for |trident|. The project aims to present a
unified design so programs feel familiar to users. As an example, while
programs could have **File**, **Main**, or **System** as their first
entry in a menu bar, **File** is used as the accepted norm for the first
category on the menu bar.

This section describes a small list of guidelines for menu and program
design in |trident|.

Any graphical program that is a fully featured utility, such as
Life Preserver \<life-preserver\>, should have a *File* menu. However,
file menus are not necessary for small widget programs or dialogue
boxes. When making a file menu, a good rule of thumb is *keep it
simple*. Most |trident| utilities do not need more than two or three
items on the file menu.

**Configure** is our adopted standard for the category containing
settings or configuration-related settings. If additional categories are
needed, check to see what other |trident| utilities are using.

File menu icons are taken from the *KDE Oxygen* or *material-design*
themes located in /usr/local/share/icons/oxygen. Use these file menu
icons so there are not too many different icons for the same function.
Table %s \<common icons\> lists some commonly used icons and their
default file names.

|trident| utilities use these buttons:

-   **Apply:** Executes settings changes and leaves the window open.
-   **Close:** Exits program without applying settings.
-   **OK:** Closes dialogue window and saves settings.
-   **Cancel:** Closes dialog window without applying settings.
-   **Save:** Keeps the current settings and closes the window.

Fully functional programs like Life Preserver \<life-preserver\> do not
use close buttons on the front of the application. Basically, whenever
there is a *File* menu, that and an x in the top right corner of the
application are used instead. Dialogues and widget programs are
exceptions to this rule.

Many users benefit from keyboard shortcuts and we aim to make them
available in every |trident| utility. Qt makes it easy to assign keyboard
shortcuts. For instance, to configure keyboard shortcuts for browsing
the **File** menu, put *&File* in the text slot for the menu entry when
making the application. Whichever letter has the & symbol in front of it
becomes the hot key. You can also make a shortcut key by clicking the
menu or submenu entry and assigning a shortcut key. Be careful not to
duplicate hot keys or shortcut keys. Every key in a menu and submenu
should have a key assigned for ease of use and accessibility.
Table %s \<shortcuts\> and Table %s \<hotkeys\> summarize the commonly
used shortcut and hot keys.

When saving an application's settings, use the *QSettings* class
whenever possible. There are two different *organizations*, depending
whether the application is running with *root* or *user* permissions.
Use **trident** as the *organization* for applications which run with
user permissions and **trident-root** for applications which are started
with root permissions via sudo. Proper use prevents the directory where
settings files are saved from being locked down by *root* applications,
allowing user applications to save and load their settings. Examples *1*
and *2* demonstrate how to use the *QSettings* class for each type of
permission.

**Example 1: User Permission Settings**

``` {.sourceCode .none}
(user application - C++ code):
QSettings settings("trident", "myapplication");
```

**Example 2: Root Permission Settings**

``` {.sourceCode .none}
(root application - C++ code):
QSettings settings("trident-root", "myapplication");
```

These resources are also helpful for developers:

-   [Qt 5.4 Documentation](http://doc.qt.io/qt-5/index.html)
-   [C++ Tutorials](http://www.cplusplus.com/doc/tutorial/)

### Documentation

|trident| is always looking for documentation contributions from its
users. The project currently has a large amount of documentation, and
the community is instrumental in keeping the information up to date and
providing tips and instructions to solve specific problems. However, the
sheer amount of documentation available coupled with the specific
documentation tools can make contributing appear daunting. Actually, the
reverse is true: **contributing to the documentation is easy!**

#### Make a Simple Documentation Change

> **tip**
>
> These instructions are for simple modifications of the |trident|
> handbook, but they also apply to the |lumina| and |sysadm|
> documentation! |lumina| documentation lives in the
> [lumina-docs](https://github.com/trident/lumina-docs) repository and
> |sysadm| guides are in
> [sysadm-docs](https://github.com/trident/sysadm-docs).

Making a documentation change can be as simple as using a web browser. A
GitHub account is required to submit patches to |trident|, so open a web
browser and log in to GitHub. Making an account is also a simple
process, but be sure to use an often checked email address, as all
communication regarding patches and pull requests are sent to this
address.

Navigate to the [trident-docs](https://github.com/trident/trident-docs)
GitHub repository. Click on the trident-handbook directory to view all
the documentation files. Open the .rst file corresponding to the chapter
needing an update. The chapter names are reflected in the title of the
.rst files. Figure %s \<docchange1\> shows the trident-docs repository
and the contents of the trident-handbook directory.

> Contents of trident-handbook

Open the desired chapter file by clicking its entry in the list.

> **tip**
>
> trident.rst is the primary index file and should be ignored.

Begin editing the file by clicking the Pencil icon in the upper right
corner above the file's text. The file moves to *edit* mode, where it is
now possible to make any necessary changes, as Figure %s \<docchange2\>
shows.

> Editing install.rst with GitHub

If making a simple change, it is recommended to avoid adjusting the
specific formatting elements and instead work within or around them.

Once satisfied, scroll to the bottom of the page and write a detailed
commit summary of the new changes. Click Propose file change (green
button), then Create pull request to submit the changes to the project.
GitHub then does an automated merge check. Click Create pull request
again to submit the change to the repository. The final step is for a
developer or project committer to review the changes, merging or asking
for more changes as necessary.

> **tip**
>
> Housekeeping: Once the pull request is merged, delete the now obsolete
> patch branch.

#### Advanced Documentation Changes

> **note**
>
> These instructions are designed for users running |trident|. Actual
> commands and workflow may change when using a different operating
> system.

Advanced changes to the |trident| documentation require an understanding
of the underlying tools and markup language. This section covers
downloading and installing the required tools and source files to build
a local copy of the documentation. It also discusses the reStructured
Text markup language and some of the specific conventions |trident| uses
in its documentation. It is recommended the contributor be familiar with
using trident and/or FreeBSD before following these instructions.

**Required Applications**

There are a few packages to install before making a local copy of the
documentation. Sphinx and its relevant extensions are the most
important.

Open |appcafe| or a command-line and download the py27-sphinx package:

[user@example] sudo pkg install py27-sphinx

Press y if prompted to continue installing the package. Next, install
the py27-sphinxcontrib-httpdomain package:

[user@example] sudo pkg install py27-sphinxcontrib-httpdomain

Be sure git is installed. |trident| installs this by default. A GitHub
account is also required to follow these instructions. Open a web
browser pointed to <https://github.com/> to create an account.

The last critical item to have on hand is a configurable text editor.
The [Lumina Text
Editor](https://lumina-desktop.org/handbook/luminautl.html#text-editor)
is a simple plaintext editor built in to |trident| which works very well
when editing .rst files, but other editors like kate and scite also
function well.

**Preparing a local copy of the GitHub repository**

Once ready with Sphinx and extensions installed, navigate to the
[trident-docs](https://github.com/trident/trident-docs) repository and
*fork* it by clicking the Fork button in the upper-right corner of the
repository. This creates a copy of the repository on the user's personal
GitHub account, allowing the user to create patches and submit pull
requests to the **upstream** (or *base*) repository.

Now there are two repositories to track on GitHub, the primary
trident-docs and the user's forked version on their personal account.

In the command line, use git clone to clone the **forked** repository:

[user@example] git clone https://github.com/[github\_user]/trident-docs.git

cd into the newly downloaded /trident-docs/ directory to continue
configuring this cloned repo.

Set up the local clone of the **forked** repository to point to the
**upstream** repository:

> **tip**
>
> GitHub also documents this procedure in two steps: [Configuring a
> remote for a
> fork](https://help.github.com/articles/configuring-a-remote-for-a-fork/)
> and [Syncing a
> fork](https://help.github.com/articles/syncing-a-fork/).

``` {.sourceCode .none}
[user@example] ~/trident-docs% git remote -v
origin  https://github.com/[github_user]/trident-docs.git (fetch)
origin  https://github.com/[github_user]/trident-docs.git (push)
[user@example] ~/trident-docs% git remote add upstream https://github.com/trident/trident-docs.git
[user@example] ~/trident-docs% git remote -v
origin  https://github.com/[github_user]/trident-docs.git (fetch)
origin  https://github.com/[github_user]/trident-docs.git (push)
upstream    https://github.com/trident/trident-docs.git (fetch)
upstream    https://github.com/trident/trident-docs.git (push)
```

This configuration allows changes made in a fork to be synced to the
original repository and changes in the original synced to the fork:

``` {.sourceCode .none}
[user@example] ~/trident-docs% git fetch upstream
[user@example] ~/trident-docs% git merge upstream/master
[user@example] ~/trident-docs% git push origin master
```

One last element to configure is to set the account identity:

``` {.sourceCode .none}
[user@example] ~/trident-docs% git config --global user.email "[you@example.com]"
[user@example] ~/trident-docs% git config --global user.name "[Your Name]"
```

This sets the *global* user name and email for git. Remove **--global**
from the command to set the value only for the trident-docs project
directory.

> **tip**
>
> It can be very useful to have two local copies of the documentation.
> One to pull in changes from "upstream" and one to make local changes
> and build test. This helps prevent merge conflicts where the local
> changes accidentally override brand new patches added to the upstream
> repository.

**Basic Git commands**

Once the local trident-docs copy is configured, there are a few general
git commands to remember when working in the directory:

-   git pull: Update the local copy from the forked GitHub repository.
-   git add [path/to\_file]: Designate a local file to stage for commit
    to the forked repository.
-   git status: Display a message showing what is staged for commit and
    other relevant information.
-   git commit: Create a patch for the forked repository. Writing a
    commit message describing the changes is always recommended.
-   git push: Send the patch created using git commit upstream to the
    user's forked repository.

GitHub provides a variety of [introductory
guides](https://guides.github.com/) for users new to its unique
workflow. It is recommended to use these guides if confused about any
stage of the commit/push/pull request process.

**Sphinx Structure**

|trident| uses the [Sphinx Documentation
Generator](http://www.sphinx-doc.org/en/stable/#) for all its
documentation. Sphinx uses
[reStructuredText](http://docutils.sourceforge.net/rst.html) source
files to generate a variety of output formats, including HTML, LaTeX,
and ePub. The Sphinx builder also tests the markup as it builds,
notifying the user of errors and approximate locations in the file.
Sphinx also supports numerous extensions and customizable elements,
including the output theme, configuration file, and other open-source
options.

> **note**
>
> The [Sphinx Project
> documentation](http://www.sphinx-doc.org/en/stable/contents.html) is
> very robust and is recommended to browse or reference it when making
> advanced changes to the |trident| documentation. The Sphinx
> [reStructuredText
> Primer](http://www.sphinx-doc.org/en/stable/rest.html) is also highly
> recommended.

The |trident| implementation of Sphinx uses a few management files and
directories to handle and build the .rst source files:

-   trident.rst: The master index file for the handbook. This file
    governs how the Table of Contents is constructed and which .rst
    files to include when starting a build.
-   conf.py: The Python configuration file for the Sphinx project. After
    the initial setup and some customization, this file is generally
    static.
-   images directory: All images used in the documentation are stored in
    this directory. Every image is .png format, and adding or removing
    an image to this directory requires updating the
    trident-docs/port-files/pkg-plist file.
-   themes directory: Houses the currently used *trident\_style* theme.
    This theme includes html, .js, and .css files, plus a custom font.
    These files govern how the documents look after an **html** build.
-   Makefile: This file houses all the specific Sphinx make commands.

**Documentation Workflow**

Once all the repository forking and configuration is done, the actual
workflow to make and submit documentation changes is straightforward:

-   cd into the local copy:
    [user@example] \~% cd /trident-docs/trident-handbook
-   Use git pull upstream master to download any changes from the
    upstream repository. Then type git merge upstream/master to add
    those changes to the local copy of the forked repository. Finally,
    use git push origin master sync these changes from the upstream
    repository the online forked repository.
-   Make any changes to the .rst files using a plaintext editor.
-   Build test the changes with make html. The builder output posts
    messages if any errors are detected. The built html files are
    viewable by opening them in a web browser:
    [user@example] \~/trident-docs/trident-handbook% firefox \_build/html/trident.html &
-   Clean the \_build directory with make clean. The contents of this
    directory are not needed in the online repositories, only when
    conducting build tests.
-   Stage a changed file for commit with git add [path/to\_file].
-   Continue changing, testing, and staging files as desired, then make
    the patch to push to the online forked repository with git commit.
    Be sure to add a descriptive commit message about the changes.
-   git push origin master to send the patch to the online forked
    repository.
-   Open a browser and navigate to the forked trident-docs repository.
    Look for the message saying "This branch is 1 commit ahead of
    trident:master" and click the Pull request button on the same line.
    GitHub checks if the branches can be automatically merged,
    displaying a *green checkmark* if everything looks good. Click
    Create pull request, add any more details to the commit message, if
    necessary, then click Create pull request again.
-   Finished! The patch is submitted for a developer or project
    maintainer to review and merge.

**Update translation files**

Once the initial patch is submitted, it is recommended to submit another
patch to update the translation files:

-   cd into the local copy and run make i18n.
-   Once the command is finished, type make clean. This removes a number
    of unnecessary files. Type git status to view the newly updated
    translation files.
-   Commit all these files to a patch with git commit -a. Use the same
    commit message as the last non-translation change, but add
    **:TRANSLATIONS** to the title. This allows the project contributors
    to more easily track when and which translation files are updated.
-   Follow the same git push and GitHub website instructions listed
    above to submit the patch to the upstream repository.

#### Documentation Conventions

This section is intended to provide references for the specific
conventions of |trident| documentation. Table %s \<specdocconv\> provides
specific conventions of the |trident| project:

> **tip**
>
> It is also recommended to open one of the handbook .rst files for
> reference.
>
>   ------------------------------------------------------------------------
>   Convention      Description               Exceptions/Examples
>   --------------- ------------------------- ------------------------------
>   70 character    Start a new line every 70 Certain elements like a long
>   lines           characters.               link or code block.
>
>   Archive old     Replaced images are moved N/A
>   images          to the archived\_images   
>                   directory.                
>
>   PNG images      All images are in the     N/A
>                   .png format.              
>
>   Update plist    Update                    N/A
>                   port-files/pkg-plist      
>                   whenever the images/      
>                   directory is changed.     
>
>   Whitespace      Remove any unnecessary    Empty lines and at the end of
>                   whitespace.               lines.
>
>   guilabel        Graphical elements:       Click Ok
>                   buttons, icons, fields,   
>                   columns, and boxes.       
>
>   menuselection   Menu selections and paths Select Foo --\> Bar
>
>   command         Commands                  Use the lcp command
>
>   file            File, volume, and dataset Locate the /etc/rc.conf file.
>                   names                     
>
>   kbd             Keyboard keys             Press the Enter key.
>
>   **Bold**        Important points          **This is important.**
>
>   *Italic*        Device names or values    Enter *127.0.0.1* in the
>                   entered into fields       address field.
>
>   samp            Command line              [user@samp] \~% ls /etc
>                   representations           
>
>   code-block      Multi-line code examples. `.. code-block:: json`
>
>   External links  Hyperlink to website      Check
>                   outside the built         [Google](https://www.google.co
>                   documentation.            m)
>
>   Internal Links  Hyperlink to an internal  See Documentation
>                   section of the            
>                   documentation             
>   ------------------------------------------------------------------------
>
Table %s \<rstmarkup\> provides a basic reference for some of the often
used elements of the reStructuredText markup language. See the [Sphinx
reStructuredText Primer](http://www.sphinx-doc.org/en/stable/rest.html)
for a more complete reference.

>   ---------------------------------------------------------------------
>   Markup Type    Description
>   -------------- ------------------------------------------------------
>   Paragraph      Test separated by one or more blank lines. All lines
>                  of the same paragraph must be left-aligned to the same
>                  level of indentation.
>
>   Inline         One asterisk around text is *italics*.
>
>   Inline         Two asterisks around text is **bold**.
>
>   Inline         Two backquotes around text is a code sample.
>
>   Inline role    Interpreted text roles: syntax is *content*.
>
>   Bullet List    Use \* for bullet list entries.
>
>   Number List    Use 1., 2., ... for a numbered list.
>
>   Nested List    Nested lists are separated from the parent items by
>                  blank lines.
>
>   Quoted par.    Add indentation.
>
>   Line blocks    Use | on the left side to preserve a quote's line
>                  breaks.
>
>   Comment        Start line with *..* and a whitespace. End the comment
>                  by adding an empty line, then starting the next line
>                  at the same level of indentation.
>
>   Escape         Use a \\\\ (backslash).
>   ---------------------------------------------------------------------
>
**Tables**

Tables are all built as grid tables. Here is an example table to copy,
paste, and rework when necessary:

``` {.sourceCode .none}
+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+
```

**Images**

Images are referenced using the numref role and internal marker:

``` {.sourceCode .none}
:numref:`Image %s <example1>` is the example syntax to introduce a
new image.

.. _example1:
.. figure:: images/example1.png
   :option: [value]

   Caption for Example1.png
```

**Admonition Boxes**

These are the specific admonition boxes used by |trident| documentation:

> **tip**
>
> A trivial shortcut or efficiency.

> **note**
>
> Useful information or reminder for accurate command use.

> **warning**
>
> Caution about potential problems introduced by the procedure or its
> misuse.

> **danger**
>
> Extreme caution. Data loss or system damage can occur.

### Advocacy

Love |trident|? Why not tell your family, friends, fellow students and
colleagues about it? You are not the only individual who prefers a
virus-free, feature-rich, and no-cost operating system. Here are some
suggestions for getting started:

-   Burn a couple of DVDs and give them away.
-   Consider giving a presentation about |trident| at a local community
    event, conference, or online. Let us know about it through our
    trident Community channels and we can help spread the word!
-   Write a personal blog detailing your journey from your first
    |trident| install experience to your most recent accomplishment. The
    blog could also be used to teach or explain how to perform tasks on
    |trident|. A regional language blog may help build the community in
    your area and to find others with similar interests.

Additional Resources
--------------------

Need more information? A number of useful resources that may aid in
using |trident| are available.

### FreeBSD Handbook and FAQ

|trident| uses FreeBSD as its underlying operating system, so nearly
everything in the [FreeBSD
Handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/)
and [FreeBSD FAQ](https://www.freebsd.org/doc/en/books/faq/) applies to
|trident| as well. Both documents are comprehensive and cover nearly
every possible task to accomplish on a FreeBSD system. They are also an
excellent resource for learning how things work under the hood of a
|trident| system.

> **note**
>
> Some configurations described in the FreeBSD Handbook already "just
> work" on a |trident| system as they have been pre-configured. In these
> instances, reading the FreeBSD Handbook section can help to learn how
> the system is configured and why it works.

### Search and Portals

Many BSD related search portals exist. If unable to find an answer from
the forums or mailing lists, try searching these websites:

-   [The
    OpenDirectory](http://dmoztools.net/Computers/Software/Operating_Systems/Unix/BSD/FreeBSD/)
-   [FreeBSD Search](https://www.freebsd.org/search/index.html)
    (includes mailing list archives, man pages, and web pages)
-   [FreeBSD News](https://www.freebsdnews.com/)
-   [About BSD](http://aboutbsd.net/)
-   [BSD Guides](http://www.bsdguides.org/guides/)
-   [Slashdot BSD](https://bsd.slashdot.org/)
-   [DistroWatch](https://distrowatch.com/)
-   [LinuxBSDos](http://linuxbsdos.com/)

### More Resources

Many BSD sites and resources may also contain useful information:

-   [The FreeBSD Diary](http://www.freebsddiary.org/)
-   [Trident YouTube
    channel](https://www.youtube.com/channel/UCyd7MaPVUpa-ueUsGjUujag)
-   [BSD YouTube channel](https://www.youtube.com/user/bsdconferences)
-   [BSD Talk](http://bsdtalk.blogspot.com/)
-   [BSD Now](http://www.bsdnow.tv/)
-   [BSD Magazine](https://bsdmag.org/) (free, monthly download)
-   [FreeBSD Journal](http://www.freebsdjournal.com/) (bi-monthly
    magazine)
-   [BSD Hacks](http://shop.oreilly.com/product/9780596006792.do) (book)
-   [The Best of FreeBSD
    Basics](http://reedmedia.net/books/freebsd-basics/) (book)
-   [Definitive Guide to
    PC-BSD](https://www.apress.com/us/book/9781430226413) (book)

**ZFS Resources**

-   [ZFS Evil Tuning
    Guide](https://www.solaris-cookbook.eu/solaris/solaris-10-zfs-evil-tuning-guide/)
-   [FreeBSD ZFS Tuning Guide](https://wiki.FreeBSD.org/ZFSTuningGuide)
-   [ZFS Best Practices
    Guide](https://documents.irf.se/get_document.php?group=Computer&docid=311)
-   [ZFS Administration
    Guide](https://docs.oracle.com/cd/E19253-01/819-5461/index.html)
-   [Becoming a ZFS Ninja
    (video)](https://blogs.oracle.com/video/becoming-a-zfs-ninja)
-   [Blog post explaining how ZFS simplifies the storage
    stack](https://blogs.oracle.com/bonwick/rampant-layering-violation)
