Using |trident|
==============

This chapter discusses using |Trident| for many common tasks. Because
Trident incorporates the |lumina| desktop environment and |sysadm| system
management utility, those projects are used for a variety of user
customization tasks such as theming, user management, and system
backups. As each of these projects have their own documentation, links
to the |lumina| and |sysadm| client handbooks are provided.

|lumina|
--------

The Lumina Desktop Environment (|lumina| for short) is a lightweight,
XDG-compliant, BSD-licensed desktop environment focused on streamlining
work efficiency with minimal system overhead. It is specifically
designed for |Trident| and FreeBSD, but has also been ported to many
other BSD and Linux operating systems. It is based on the Qt graphical
toolkit and the Fluxbox window manager, and uses a small number of X
utilities for various tasks, such as numlockx and xscreensaver.

|lumina|'s features include:

-   Very little system overhead.
-   Does not require any of the desktop implementation frameworks such
    as DBUS, policykit, consolekit, systemd, or HALD.
-   Provides many utilities \<luminautl.html\> for configuring the
    desktop environment.
-   Provides an interface design based on
    plugins \<luminaplugins.html\>. The user can make their desktop as
    light or heavy as they wish by choosing which plugins to have
    running on their desktop and panels. This plugin-based system is
    similar to Android or other modern operating systems.
-   A single, easy-to-use Lumina Configuration \<luminaconfig.html\>
    utility controls all the different configuration options for the
    desktop in one location.
-   Intelligent *favorites* system for creating quick shortcuts to
    applications, files, and directories.
-   ZFS file restore functionality with
    Insight File Manager \<luminautl.html\#insight-file-manager\>.
-   Multi-monitor support includes the
    Lumina Xconfig \<luminautl.html\#xconfig\> graphical utility for
    adding or removing monitors from the |lumina| session.
-   Simple system controls \<luminaintro.html\#initial-settings\>
    through the system tray for configuring audio volume, screen
    brightness, battery status/notifications, and workspace switching.
-   Total system search capabilities through the resource friendly
    Lumina Search \<luminautl.html\#lumina-search\> utility.
-   Screenshot functionality through
    Lumina Screenshot \<luminautl.html\#screenshot\>, which is tied to
    the Print Screen key by default.

Refer to the [Lumina Handbook](https://lumina-desktop.org/handbook/) for
detailed descriptions of every element of |lumina|.

These next sections describe each element of |Trident| that is managed by
|lumina| and provides links to the relevant section of the Lumina
handbook.

### Desktop Configuration

The Lumina Configuration \<luminaconfig.html\> utility allows the user
to customize virtually every aspect of the desktop. It is opened by
right-clicking the desktop, then selecting
Preferences --\> All Desktop Settings. These are the configurable
elements using this utility:

Appearance \<luminaconfig.html\#appearance\>: Adjust theming,
wallpapers, and windows. Theming includes default font and size, theme
template, color scheme, icon pack, and mouse cursors. There is also an
option to adjust the application themes, either manually or by applying
a downloaded theme engine.

Modifying windows includes effects, compositing, and default settings.
An Advanced Editor is also provided for the user to manually modify any
existing value.

Desktop Defaults \<luminaconfig.html\#desktop-defaults\>: Customize
which applications are associated with specific filetypes, the default
file manager, virtual terminal, web browser, and e-mail client. Choose
packages to autostart when the system boots. Also adjust the default
keyboard shortcuts for the system.

Interface Configuration \<luminaconfig.html\#interface-configuration\>:
Customize widgets for the Desktop, the appearance and options in the
right-click Menu, and appearance, number, and options for any system
panels.

System Settings \<luminaconfig.html\#system-settings\>: Central location
for all configurable system utilities. |lumina| updates this category as
new utilities are added and removed.

User Settings \<luminaconfig.html\#user-settings\>: General settings for
the user's desktop session. Includes adjusting time/date, user icons,
chime options, and all localization options.

### Utilities included with Trident

To provide a simple, but fully featured user experience immediately "out
of box", |Trident| includes several utilities built directly into
|lumina|.

Archiver \<luminautl.html\#archiver\>: Provides file compression and
decompression services.

Calculator \<luminautl.html\#calculator\>: Basic calculator with
scientific options and advanced functions.

Insight File Manager \<luminautl.html\#insight-file-manager\>: Browse
and modify files on a per-directory basis.

File Information \<luminautl.html\#file-information\>: Displays specific
information about a specified file or directory, including permissions,
ownership, size, and date of last modification.

Information \<luminautl.html\#information\>: Provides more information
about the installed version of |lumina|.

Open \<luminautl.html\#open\>: This utility assists the user in finding
programs to open specific files or URLs. It can also be used to set the
default application for specific file types.

Screenshot \<luminautl.html\#screenshot\>: A very simple utility to take
screenshots of the desktop, single windows, or designated areas of a
screen. Screenshots can be saved as .png files.

Search \<luminautl.html\#lumina-search\>: Find and launch applications
or quickly search for files and directories.

Text Editor \<luminautl.html\#text-editor\>: Plain text editor with
customizable settings and built in rules for specific file types.

Xconfig \<luminautl.html\#xconfig\>: Graphical front-end to the xrandr
command line utility. Manages attached monitors, allowing the user to
add, alter the position, and configure screens.

Network Manager
---------------

During installation, |Trident| configures any connected Ethernet
interfaces to use DHCP and provides a screen to
Connect to a Wireless Network. In most cases, this means connected
interfaces should "just work" whenever using a |Trident| system.

After installation, a wireless configuration icon appears in the system
tray if |Trident| detects a supported wireless card. Hover over the
wireless icon shown in Figure %s \<network1\> to see an indication if
the interface is associated and more information regarding the IP
address, IPv6 address, SSID, connection strength, connection speed, MAC
address, and type of wireless device.

![System Tray Wireless Information](images/network1.png)

If you right-click the wireless icon, a list of detected wireless
networks displays. Click the name of a network to associate with it. The
right-click menu also provides options to configure the wireless device,
start the Network Manager, restart the network (useful to renew your
DHCP address), route the network connection through Tor (to browse the
Internet anonymously as described in Tor Mode), and close the Network
Monitor so the icon no longer shows in the system tray.

To view or manually configure a network interface, click
Start the Network Manager within |sysadm| or type sudo pc-netmanager. If
a new device has been inserted, such as a USB wireless interface, a
pop-up message opens when Network Manager starts. This message indicates
the name of the new device and asks if you want to enable it. Click Yes
and the new device is displayed with the list of network interfaces that
|Trident| recognizes. In the example seen in Figure %s \<network2\>, the
system has one Intel Ethernet interface that uses the **em** driver and
an Intel wireless interface that uses the **wlan** driver.

![Network Manager](images/network2.png)

The rest of this section describes each tab of the Network Manager
utility and demonstrates how to view and configure the network settings
for both Ethernet and wireless devices.

### Network Devices

If you highlight an Ethernet interface in the Devices tab and either
click Configure or double-click the interface name, the screen shown in
Figure %s \<network3\> appears.

![Network Settings for an Ethernet Interface](images/network3.png)

There are two ways to configure an Ethernet interface:

1.  **Use DHCP:** This method assumes your Internet provider or network
    router assigns addressing information automatically using the DHCP
    protocol. Most networks are built in this manner. This method is
    recommended as it should "just work".
2.  **Manually type in the IP addressing information:** This method
    requires an understanding of the basics of TCP/IP addressing or
    knowledge of which IP address to use on your network. If you do not
    know which IP address or subnet mask to use, ask your Internet
    provider or network administrator.

By default, |Trident| attempts to obtain an address from a DHCP server.
If you wish to manually type in your IP address, check
Assign static IP address. Type in the IP address, using the right arrow
key or the mouse to move between octets. Then, double-check the subnet
mask (**Netmask**) is the correct value. If not, change it again.

If the Ethernet network uses 802.1x authentication, check
Enable WPA authentication, which enable the Configure WPA button. Click
this button to select the network and input the authentication values
required by the network.

By default, Disable this network device is unchecked. If this checkbox
is filled, |Trident| immediately stops the interface from using the
network. The interface remains inactive until this checkbox is
unchecked.

The Advanced tab, seen in Figure %s \<network4\>, allows advanced users
to manually input a MAC address \<MAC\_address\> or
IPv6 address \<IPv6\_address\>. Both boxes should remain checked in
order to automatically receive these addresses, unless you are an
advanced user with reason to change the default MAC or IPv6 address and
an understanding of how to input an appropriate replacement address.

![Ethernet Interface Network Settings - Advanced](images/network4.png)

The Info tab, seen in Figure %s \<network5\>, displays the current
network address settings and some traffic statistics.

![Ethernet Interface Network Settings - Info](images/network5.png)

If any changes are made within any of the tabs, click Apply to activate
them. Click OK when finished to return to the main Network Manager
window.

Repeat this procedure for each desired network interface.

### Wireless Adapters

If the wireless interface does not automatically associate with a
wireless network, the wireless profile containing the security settings
required by the network will need to be configured. Double-click the
wireless icon in the system tray or highlight the wireless interface
displayed in the Devices tab of Network Manager and click Configure.
Figure %s \<network6\> demonstrates this system's wireless interface is
currently associated with the wireless network listed in the
Configured Network Profiles section.

![Wireless Configuration](images/network6.png)

To associate with a wireless network, click Scan to view a list of
connectable wireless networks. Highlight the desired network to
associate with and click +Add Selected. If the network requires
authentication, a pop-up window prompts you for the authentication
details. Input the values required by the network, then click Close.
|Trident| then adds an entry for the network in the
Configured Network Profiles section.

If the network is hidden, click +Add Hidden, input the name of the
network in the pop-up window, and click OK.

If multiple networks are added, use the arrow keys to place them in the
desired connection order. |Trident| attempts to connect to networks in
order from first to last in the connection list. When prioritizing
connections, click Apply. A pop-up message then indicates |Trident| is
restarting the network. Next, an IP address and status of **associated**
appears when hovering over the wireless icon in the system tray. If this
does not happen, double-check for errors in the configuration values and
read the Troubleshooting section on Network Help.

|Trident| supports the types of authentication shown in
Figure %s \<network7\>. Access this screen and change authentication
settings by highlighting an entry in the Configured Network Profiles
section and clicking Edit.

![Configuring Wireless Authentication Settings](images/network7.png)

This screen provides configuration of different types of wireless
security:

-   **Disabled:** If the network is open, no additional configuration is
    required.
-   **WEP:** This type of network can be configured to use either a hex
    or a plaintext key and Network Manager will automatically select the
    type of detected key. If WEP is pressed, then Configure, the screen
    in Figure %s \<network8\> appears. Type the key into both
    Network Key boxes. If the key is complex, check Show Key to ensure
    the passwords are matching and correct. Uncheck this box when
    finished to replace the characters in the key with bullets. A
    wireless access point using WEP can store up to 4 keys and the
    number in the key index indicates which desired key to use.

    ![WEP Security Settings](images/network8.png)

-   **WPA Personal:** This type of network uses a plaintext key. If you
    click WPA Personal then Configure, the screen shown in
    Figure %s \<network9\> appears. Type in the key twice to verify it.
    If the key is complex, check Show Key to ensure the passwords match.

    ![WPA Personal Security Settings](images/network9.png)

-   **WPA Enterprise:** If you click WPA Enterprise then Configure, the
    screen shown in Figure %s \<network10\> appears. Select the
    EAP Authentication Method, input the EAP identity, browse for the CA
    certificate, client certificate and private key file, and input and
    verify the password.

    ![WPA Enterprise Security Settings](images/network10.png)

> **note**
>
> If unsure which type of encryption is being used, ask the person who
> setup the wireless router. They should also be able to provide the
> value of any settings seen in these configuration screens.

To disable this wireless interface, check Disable this wireless device
in the General tab for the device. This setting is helpful when
temporarily preventing the wireless interface from connecting to
untrusted wireless networks.

The Advanced tab, seen in Figure %s \<network11\>, allows configuring
several options:

-   **Custom MAC address:** This setting is for advanced users and
    requires Use hardware default MAC address to be unchecked.
-   **Interface receiving IP address information:** If the network
    contains a DHCP server, check Obtain IP automatically (DHCP).
    Otherwise, input the IP address and subnet mask to use on the
    network.
-   **Country code:** This setting is not required if in North America.
    For other countries, check Set Country Code and select your country
    from the drop-down menu.

![Wireless Interface - Advanced](images/network11.png)

The Info tab, seen in Figure %s \<network12\>, shows the current network
status and statistics for the wireless interface.

![Wireless Interface - Info](images/network12.png)

### Network Configuration (Advanced)

The Network Configuration (Advanced) tab of the Network Manager is seen
in Figure %s \<network13\>. The displayed information is for the
currently highlighted interface. To edit these settings, ensure the
interface to configure is highlighted in the Devices tab.

![Network Configuration - Advanced](images/network13a.png)

If the interface receives its IP address information from a DHCP server,
this screen allows viewing of the received DNS information. To override
the default DNS settings or set them manually, check Enable Custom DNS.
You can then set:

-   **DNS 1:** The IP address of the primary DNS server. If unsure which
    IP address to use, click Public servers to select a public DNS
    server.
-   **DNS 2:** The IP address of the secondary DNS server.
-   **Search Domain:** The name of the domain served by the DNS server.

To change or set the default gateway, check Enable Custom Gateway box
and input the IP address of the desired gateway.

Several settings can be modified in the IPv6 section:

-   **Enable IPv6 support:** If this box is checked, the specified
    interface can participate in IPv6 networks.
-   **IPv6 gateway:** The IPv6 address of the default gateway used on
    the IPv6 network.
-   **IPv6 DNS 1:** The IPv6 address of the primary DNS server used on
    the IPv6 network. If unsure which IP address to use, click
    Public servers to select a public DNS server.
-   **IPv6 DNS 2:** The IPv6 address of the secondary DNS server used on
    the IPv6 network.

The Misc section has more options to configure:

-   **System Hostname:** The name of your computer. It must be unique on
    your network.
-   **Domain Name:** If the system is in a domain, specify it here.
-   **Enable wireless/wired failover via lagg0 interface:** This
    interface allows seamless switching between using an Ethernet
    interface and a wireless interface. Check the box to enable this
    functionality.

> **note**
>
> Some users experience problems using lagg. If you have problems
> connecting to a network using an interface which previously worked,
> uncheck this box and remove any references to lagg from /etc/rc.conf.

If any changes are made within this window, click Apply to save them.

### Proxy Settings

The Proxy tab, shown in Figure %s \<network14\>, is used when the
network requires going through a proxy server to access the Internet.

![Proxy Settings Configuration](images/network14.png)

Check Proxy Configuration to activate the settings. Some settings can be
configured in this screen:

-   **Server Address:** Enter the IP address or hostname of the proxy
    server.
-   **Port Number:** Enter the port number used to connect to the proxy
    server.
-   **Proxy Type:** Choices are **Basic** (sends the username and
    password unencrypted to the server) and **Digest** (never transfers
    the actual password across the network, but instead uses it to
    encrypt a value sent from the server). Do not select **Digest**
    unless the proxy server supports it.
-   **Specify a Username/Password:** Check this box and input the
    username and password if they are required to connect to the proxy
    server.

Proxy settings are saved to the /etc/profile and /etc/csh.cshrc files so
they are available to both the |Trident| utilities and any application
using fetch.

Applications not packaged with the operating system, such as web
browsers, may require configuring proxy support using that application's
configuration utility.

If you apply any changes to this tab, a pop-up message warns the user
may have to log out and back in for the proxy settings to take effect.

### Configuring a Wireless Access Point

Right-click the entry for a wireless device, as seen in
Figure %s \<network15\>, and choose Setup Access Point.

![Setup Access Point](images/network15.png)

Figure %s \<network16\> shows the configuration screen if
Setup Access Point is selected.

![Access Point Basic Setup](images/network16.png)

The Basic Setup tab of this screen contains two options:

-   **Visible Name:** This is the name appearing when users scan for
    available access points.
-   **Set Password:** Setting a WPA password is optional, though
    recommended to only allow authorized devices to use the access
    point. If used, the password must be a minimum of 8 characters.

Figure %s \<network17\> shows the Advanced Configuration (optional)
screen.

![Access Point Advanced Setup](images/network17.png)

The settings in this screen are optional and allow for fine-tuning the
access point's configuration:

-   **Base IP:** The IP address of the access point.
-   **Netmask:** The associated subnet mask for the access point.
-   **Mode:** Available modes are **11g** (for 802.11g), **11ng** (for
    802.11n on the 2.4-GHz band), or **11n** (for 802.11n).
-   **Channel:** Select the channel to use.
-   **Country Code:** The two letter country code of operation.

### Tor Mode

Tor mode uses [Tor](https://www.torproject.org/),
[socat](http://www.dest-unreach.org/socat/), and a built-in script which
automatically creates the necessary firewall rules to enable and disable
Tor mode at the user's request. While in Tor mode, the firewall
redirects all outgoing *port 80* (HTTP), *443* (HTTPS), and DNS traffic
through the Tor transparent proxy network.

To start Tor mode, right-click the network icon in the system tray and
check Route through TOR. Enter your password via the pop-up shown in
Figure %s \<tor1\>. If activated correctly, |Trident| opens a new browser
window directed to <https://check.torproject.org>

![Enabling Tor Mode](images/tor1.png)

If you have never used the Tor network before, it is recommended to
review the [Tor FAQ](https://www.torproject.org/docs/faq.html.en).

The system remains in Tor mode until manually disabled. To disable Tor
mode, right-click the network icon and uncheck Route through Tor.

To enable and disable Tor mode from the command line or on a desktop
with no system tray, use these commands:

-   sudo enable-tor-mode enables tor mode.
-   sudo disable-tor-mode disables tor mode.

|sysadm|
--------

Beginning with |Trident| 11, most of the system management utilities
previously available in the |pcbsd| Control Panel have been rewritten to
use the |sysadm| [API](https://api.sysadm.us/getstarted.html). This API
is designed to simplify managing any FreeBSD, |Trident| desktop, or
|Trident| server system over a secure connection from any operating
system with the |sysadm| application installed. |sysadm| is built into
|Trident| as the SysAdm: Control Panel.

The |sysadm| [Client Handbook](https://sysadm.us/handbook/client/) is
recommended for new |Trident| users, while the
[Server](https://sysadm.us/handbook/server/) and [API
Reference](https://api.sysadm.us/) guides are available for advanced
users.

The rest of this section describes the elements of Trident controlled by
SysAdm, providing links to the relevant SysAdm documentation:

> **Application Management**

AppCafe \<appcafe\> is a graphical interface for installing and managing
FreeBSD packages. These are pre-built applications tested for use with a
FreeBSD-based operating system. Open |appcafe| by clicking the Start
button (lower-left), then Control Panel --\> AppCafe.

|appcafe| breaks popular applications into a few categories and provides
search functionality for users looking for a specific application. Users
can also browse through a list of all currently installed applications
and view more details or delete them from the system.

Update Manager \<update-manager\> is a graphical interface for keeping
both |Trident| and its installed applications up to date. Users can check
for updates, switch between the STABLE and UNSTABLE tracks of Trident,
configure automatic updating, and view the log files of previous
updates.

See Updating Trident for more details about using the Update Manager.

> **SysAdm Server Settings**

Managing Remote Connections \<managing-remote-connections\> provides
instructions to create and manage SSL keys or certificate bundles.

> **System Management**

|sysadm| provides the "core" for managing |Trident|:

Boot Environment Manager \<boot-environment-manager\>: Create and manage
ZFS Boot Environments (BEs). Boot Environments provide a "point-in-time"
backup for the system and are highly recommended. Options to *create*,
*clone*, *delete*, *rename*, *mount*, *unmount*, and *activate* a BE are
available.

Mouse Settings \<mouse-settings\>: Tool for adjusting the settings of a
connected mouse. Acceleration, DPI, right or left hand, drift, button
emulation, and scrolling are all adjustable.

Firewall Manager \<firewall-manager\>: This is used to configure all
ports and firewalls for |Trident|. Options to *open* and *close* ports
are available, including adjusting the firewall's autostart settings.

Service Manager \<service-manager\>: This allows viewing and configuring
all the system's installed services. There are options to *start*,
*stop*, and *restart* services. Additional tunables to adjust automatic
starting of services are provided.

Task Manager \<task-manager\>: A graphical window into system resource
usage and a list of all running applications. This provides details
about what is currently happening on the system and allows the user to
stop any currently running process.

User Manager \<user-manager\>: This utility controls users and groups.
There are Standard and Advanced views for both users and groups, and all
options for creating new users and groups are provided.

PersonaCrypt \<personacrypt\> security can also be added to user
accounts. This encrypts a user account so it only becomes accessible
with the proper password or by plugging in an associated USB drive.

> **Utilities**

Life Preserver \<life-preserver\> is the only utility currently included
with |Trident|. This utility is used for system backups with
ZFS \<ZFS Overview\> snapshots. Life Preserver provides easy management,
replication, and scheduling of ZFS snapshots.

Files and File Sharing
----------------------

Several file managers are available for installation using |appcafe|.
Table %s \<filemanagers\> provides an overview of several popular file
managers. To launch an installed file manager, type its name as it
appears in the Application column. To install the file manager, use
|appcafe| to install the package name listed in the Install column. To
research a file manager's capabilities, start with the URL listed in its
Screenshot column.

When working with files on a |Trident| system, save your files to your
home directory. Since most of the files outside your home directory are
used by the operating system and applications, you should not delete or
modify any files outside of your home directory unless confident in what
you are doing.

Table %s \<dirstructure\> summarizes the directory structure found on a
|Trident| system. man hier explains this directory structure in more
detail.

|Trident| provides built-in support for accessing Windows shares, meaning
you only have to decide which utility you prefer to access existing
Windows shares on your network.

Table %s \<windows shares utils\> summarizes some of the available
utilities.

Managing System Services and Daemons
------------------------------------

|Trident| now uses [OpenRC](https://wiki.gentoo.org/wiki/Project:OpenRC)
to manage system services. OpenRC is an integral component of the
|Trident| operating system, and is a major point of difference between
|Trident| and FreeBSD. This section is intended to provide detailed
information about system service management in |Trident|.

### OpenRC in |Trident| compared with rc

Table %s \<trfbsdrc\> serves as a quick summary and series of working
examples contrasting the FreeBSD rc system and OpenRC in |Trident|.

> **warning**
>
> The user may find leftover RC files during the |Trident| migration to
> OpenRC. These files do not work with OpenRC and are intended to be
> removed both from the source tree and via pc-updatemanager when all
> functionality is successfully migrated. If discovered, **do not**
> attempt to use these leftover files.

### Service Management in OpenRC

#### Runlevels

Traditionally, FreeBSD operates in single- and multi-user modes.
However, OpenRC offers the ability to define **runlevels**. An OpenRC
**runlevel** is a grouping of services, nothing more. Any number of
system services can be associated with a given runlevel. In |Trident|,
there are two main preconfigured runlevels: **boot** and **default**.
The **default** runlevel is analogous to the FreeBSD multi-user mode,
and is associated with the *Normal Bootup* option of the |Trident|
bootloader.

> **note**
>
> No OpenRC runlevels are executed if the system is booted into
> single-user mode (see Figure %s \<boot1\>.)

Runlevels are defined by subdirectories of /etc/runlevels; all
associations between services and runlevels can be shown by running the
command:

\$ rc-update show -v

OpenRC has a few ordered runlevels in |Trident|. In order of execution:

1.  **sysinit**: Used for OpenRC to initialize itself.
2.  **boot**: Starts most base services from /etc/init.d/.
3.  **default**: Services started by ports are added here.

> **note**
>
> Services added by ports cannot be added to *boot* or *sysinit*.

OpenRC allows users to add services in the prefix location to the *boot*
runlevel. These services are started before the /usr filesystem is
mounted. Finally, there is a *shutdown* runlevel reserved for a few
services like savecore or pc-updatemanager, which installs updates at
shutdown.

When a service is added to a runlevel, a symlink is created in
/etc/runlevels. When a service is started, stopped, or changed to
another state, a symlink is added to /libexec/rc/init.d/, as seen in
this example:

``` {.sourceCode .none}
[tmoore@Observer] ~% ls /libexec/rc/init.d/
daemons exclusive inactive scheduled starting wasinactive
depconfig failed options softlevel stopping
deptree hotplugged prefix.lock started tmp
```

#### Services and Runlevels

OpenRC includes options to *start*, *stop*, *add*, or *delete* services
from runlevels as seen in Table %s \<rcbootserv\>. Most of these actions
can be accomplished using the Service Manager \<service-manager\> built
into |sysadm|. Individuals familiar with the FreeBSD service command may
notice some similarities between some of these commands.

#### Writing OpenRC Services

OpenRC has a dependency based init system. As an example, examine the
SysAdm service, which needs *network*. Here are the contents of the
/usr/local/etc/init.d/sysadm *depend* section:

``` {.sourceCode .none}
depend() {
need net
after bootmisc
keyword -shutdown
}
```

SysAdm requires *network* (**need net**), which is the nickname of the
/etc/init.d/network service defined by *provide in network*. SysAdm also
starts **after** *bootmisc*. If you don’t want the restarting *network*
to restart SysAdm, then *net* is unnecessary. To start SysAdm after
*network*, then add *network to the actual name of the script inafter
bootmisc*\*.

Here are the contents of /etc/init.d/network:

``` {.sourceCode .none}
depend()
{
provide net
need localmount
after bootmisc modules
keyword -jail -prefix -vserver -stop
}
```

The *provide* option sets the service nickname to *net*. *Need* means
restarting *localmount* restarts *network*. *After* indicates the
service starts after *bootmisc* and *modules*. For example, the keyword
*-jail* option says this service doesn't run in a jail, prefix, or any
of the other options shown.

There is also a cache directory under /libexec/rc. This keeps a
dependencies cache that is only updated when those dependencies change.
Several other directories exist for other binaries and special binaries
used by OpenRC functions.

For more creation options for OpenRC compatible init scripts, type
man openrc-run in a CLI.

### RC Defaults

> **note**
>
> RC Defaults are subject to change during development.

|Trident| and FreeBSD now have very different rc defaults.

**Trident OpenRC Defaults**

The entire [Trident rc.conf
file](https://github.com/Trident/freebsd/blob/drm-next-4.7/etc/defaults/rc.conf)
is viewable on GitHub.

``` {.sourceCode .none}
# Global OpenRC configuration settings

# Set to "YES" if you want the rc system to try and start services
# in parallel for a slight speed improvement. When running in parallel
# we prefix the service output with its name as the output will get
# jumbled up.
# WARNING: whilst we have improved parallel, it can still potentially
# lock the boot process. Don't file bugs about this unless you can
# supply patches that fix it without breaking other things!
#rc_parallel="NO"

# Set rc_interactive to "YES" and you'll be able to press the I key
# during boot so you can choose to start specific services. Set to "NO"
# to disable this feature. This feature is automatically disabled if
# rc_parallel is set to YES.
#rc_interactive="YES"

# If we need to drop to a shell, you can specify it here.
# If not specified we use $SHELL, otherwise the one specified in
# /etc/psswd, otherwise /bin/sh
```

**FreeBSD RC Defaults**

The entire [FreeBSD rc.conf
file](https://github.com/freebsd/freebsd/blob/master/etc/defaults/rc.conf)
is available online.

``` {.sourceCode .none}
#!/bin/sh

# This is rc.conf - a file full of useful variables that you can set
# to change the default startup behavior of your system.  You should
# not edit this file!  Put any overrides into one of the
# ${rc_conf_files} instead and you will be able to update these
# defaults later without spamming your local configuration information.
#
# The ${rc_conf_files} files should only contain values which override
# values set in this file.  This eases the upgrade path when defaults
# are changed and new features are added.
#
# All arguments must be in double or single quotes.
#
# For a more detailed explanation of all the rc.conf variables, please
# refer to the rc.conf(5) manual page.
#
# $FreeBSD$

##############################################################
```

The |Trident| rc.conf file is smaller because rc.conf is now primarily
used for tuning OpenRC behavior. By default, |Trident| uses 3 elements,
documented in Table %s \<orcpritun\>.

Table %s \<rcuprnlvl\> lists services and their default runlevels in
|Trident|.

### Tuneables

Table %s \<orcalltun\> shows all other tunables enabled on a clean
|Trident| installation. Many of these tunables continue to work in
/etc/rc.conf to ensure a smoother migration for existing users to
upgrade. The eventual target locations for these services are also
listed.

> **note**
>
> These migration targets are estimates and subject to change.

### OpenRC Install Scripts

There are number of scripts used for older |Trident| systems and new
installations. These are listed below.

#### One-time Migration Script

A one time migration script is available for |Trident| installations
dated 10-28-16 or older still using the legacy FreeBSD *rc* system:

> **note**
>
> This block is truncated from the [original
> file](https://github.com/Trident/Trident-core/blob/master/xtrafiles/local/bin/migrate_rc_openrc)

``` {.sourceCode .none}
#!/bin/sh

if [ ! -e /etc/rc.conf ] ; then
  exit 0
fi

. /etc/rc.conf

for var in `set | grep "_enable="`
do
  key=`echo $var | cut -d '=' -f 1 | sed 's|_enable||g'`
  val=`echo $var | cut -d '=' -f 2`
  if [ "$val" != "YES" ] && [ "$val" != "NO" ] ; then continue; fi
  if [ "$val" = "NO" ] && [ -e "/etc/runlevels/default/$key" ] ; then
      echo "Deleting OpenRC service for $key to default runlevel..."
      rc-update delete $key default
  fi
  if [ -e "/etc/init.d/$key" -o -e "/usr/local/etc/init.d/$key" ] ; then
    if [ -e "/etc/runlevels/default/$key" ] ; then
      echo "OpenRC service for $key already enabled, skipping.."
```

With this migration, rc.conf.Trident, located in /etc/, has been phased
out of |Trident| and is automatically removed from legacy installs dated
10-28-16 and older by pc-updatemanger:

This script defines a list of services such as *PCDM* designated to boot
by default on a desktop. It also defines what drivers to load on a
desktop. This is now accomplished when the *Trident-desktop* or
*Trident-server* package is installed using sysrc or other methods. Now
there is no need to keep an extra overlay file to accomplish this
behaviour.

#### |Trident| Desktop pkg-install Script

> **note**
>
> This is an excerpt from the |Trident| Desktop pkg-install file,
> available online:
> <https://github.com/Trident/Trident-desktop/blob/master/port-files/pkg-install>

``` {.sourceCode .none}
#!/bin/sh
# Script to install preload.conf

PREFIX=${PKG_PREFIX-/usr/local}

if [ "$2" != "POST-INSTALL" ] ; then
   exit 0
fi

# If this is during staging, we can skip for now
echo $PREFIX | grep -q '/stage/'
if [ $? -eq 0 ] ; then
   exit 0
fi

# REMOVEME - Temp fix to ensure i915kms is loaded on upgraded systems
# 8-29-2016
if [ -e "/etc/rc.conf.Trident" ] ; then
  set +e
  grep -q "i915kms" /etc/rc.conf.Trident
```

#### Trident Server pkg-install script

> **note**
>
> This is an excerpt from the |Trident| Server pkg-install file,
> available on GitHub:
> <https://github.com/Trident/Trident-server/blob/master/port-files/pkg-install>

``` {.sourceCode .none}
#!/bin/sh
# Script to install preload.conf

PREFIX=${PKG_PREFIX-/usr/local}

if [ "$2" != "POST-INSTALL" ] ; then
   exit 0
fi

# If this is during staging, we can skip for now
echo $PREFIX | grep -q '/stage/'
if [ $? -eq 0 ] ; then
   exit 0
fi

# Copy over customizations for Trident
  install -m 644 ${PREFIX}/share/Trident/conf/loader.conf.Trident /boot/loader.conf.Trident
  install -m 644 ${PREFIX}/share/Trident/conf/brand-Trident.4th /boot/brand-Trident.4th
  install -m 644 ${PREFIX}/share/Trident/server-defaults/etc/conf.d/modules /etc/conf.d/modules/
```

The typical nginx\_enable=”YES” is no longer used to enable services.
Instead, rc-update adds or deletes services from runlevels. The one time
migration script automatically adds previously defined user services to
the OpenRC default runlevel. Leftover lines can be removed after
migration.

### Updating a Port's Makefile

There are many required updates to adjust each port's Makefile to the
new format, **USE\_OPENRC\_SUBR=**. However, these are to be changed
only when each service file has the new OpenRC ready format:

> **note**
>
> This is an excerpt from the |Trident| openrc-dbus.in file, which is
> available on the |Trident| [freebsd-ports GitHub
> repository](https://github.com/Trident/freebsd-ports/blob/Trident-master/devel/dbus/files/openrc-dbus.in)

``` {.sourceCode .none}
#!/sbin/openrc-run
# Copyright (c) 2007-2015 The OpenRC Authors.
# See the Authors file at the top-level directory of this distribution
# and https://github.com/OpenRC/openrc/blob/master/AUTHORS
#
# This file is part of OpenRC. It is subject to the license terms in
# the LICENSE file found in the top-level directory of this
# distribution and at
# https://github.com/OpenRC/openrc/blob/master/LICENSE.
# This file may not be copied, modified, propagated, or distributed
# except according to the terms contained in the LICENSE file.

command=/usr/local/bin/dbus-daemon
pidfile=/var/run/dbus/pid
command_args="${dbusd_args---system}"
name="Message Bus Daemon"

depend()
{
    need localmount
    after bootmisc
}
```

Here is an example from FreeBSD of *dbus* using the legacy rc script
format:

> **note**
>
> This is an excerpt from the legacy FreeBSD dbus.in file, which is
> available online:
> <https://github.com/freebsd/freebsd-ports/blob/master/devel/dbus/files/dbus.in>

``` {.sourceCode .none}
#!/bin/sh
#
# $FreeBSD$
#
# PROVIDE: dbus
# REQUIRE: DAEMON ldconfig
#
# Add these lines to /etc/rc.conf to enable the D-BUS messaging system:
#
# dbus_enable="YES"
#

. /etc/rc.subr
. %%GNOME_SUBR%%

dbus_enable=${dbus_enable-${gnome_enable}}
dbus_flags=${dbus_flags-"--system"}

name=dbus
rcvar=dbus_enable
```

Several developers are working on the thousands of instances as quickly
as possible. Anyone can begin transitioning to defining all service
configurations in /etc/conf.d/, if desired. All configuration files
should reside in that directory with the name of the service for the
configuration file itself. For example, *nginx* is /etc/conf.d/nginx.

Generally, usage of /etc/rc.conf is minimized. Tweaking the default
OpenRC configuration parameters is recommended only for advanced users.
It is still possible to use service configurations through /etc/rc.conf,
but this file is unusable for enabling or disabling services for
startup.

Flash Plugin
------------

|Trident| supports using a Flash plugin for those browsers/applications
that use Flash. To begin using this plugin, search for and install
"linux-flashplayer" using |appcafe|. Alternately, type
[samp@examp] \~% sudo pkg install linux-flashplayer in a command line
and enter the root password when requested.

The "nspluginwrapper" is also required when using Flash. Install it with
|appcafe| or by typing [samp@examp] \~% sudo pkg install nspluginwrapper
in a command line.

Once *linux-flashplayer* and *nspluginwrapper* are installed, configure
them by opening a command line and typing this command:

``` {.sourceCode .none}
% nspluginwrapper -v -a -i

Auto-install plugins from /usr/local/lib/browser_plugins
Looking for plugins in /usr/local/lib/browser_plugins
Auto-install plugins from /usr/local/lib/browser_plugins/linux-flashplayer
Looking for plugins in /usr/local/lib/browser_plugins/linux-flashplayer
Install plugin /usr/local/lib/browser_plugins/linux-flashplayer/libflashplayer.so
  into /usr/home/tmoore/.mozilla/plugins/npwrapper.libflashplayer.so
Auto-install plugins from /usr/home/tmoore/.mozilla/plugins
Looking for plugins in /usr/home/tmoore/.mozilla/plugins
```

In this example, Flash is configured and ready for use with the Firefox
browser. To confirm Flash is usable, open Firefox and type
**<about:plugins>** in the address bar. An *Installed plugins* page
displays, listing *Shockwave Flash* an installed plugin. See
Figure %s \<flash1\> as an example of Firefox with Flash installed.

!["<about:plugins>" Example](images/flash1.png)

Automounter
-----------

> **tip**
>
> The *Mount Tray* has been replaced by the new **Automounter**.

The automounter, based on the devd and automount utilities, facilitates
mounting and unmounting USB storage devices and optical media. It also
conforms to an **XDG** standard to allow the addition of new features.
The automounter is part of the default |Trident| installation, but is
generally invisible until a new device is attached to the system.

Currently, the automounter ignores internal hard drives (sata, ide) and
networking shares. It does support many different filesystems:

-   cd9660
-   exFAT (Requires mount.exfat-fuse. Possible intermittent detection
    issues.)
-   ext2
-   ext4 (Requires ext4fuse)
-   FAT32
-   MSDOSFS
-   MTPfs (Requires simple-mtpfs)
-   NTFS (Requires ntfs-3g)
-   ReiserFS
-   UDF
-   UFS
-   XFS

> **warning**
>
> Linux based filesystems may have some limitations. See
> Table %s \<filesys support\> for more details.

To engage the automounter, attach a USB storage device or insert optical
media to the system. The automounter detects the device by ID and adds
icons to the desktop, as seen in Figure %s \<automnt1\>:

![USB icons added to desktop via the automounter. Hovering over the icon
displays the actual device name and filesystem
type.](images/automnt1.png)

> **tip**
>
> The appearance of these icons do **not** mean the device is mounted.
> Devices are only mounted when the user begins to interact with the
> device.

Either navigating to a device or beginning copy operations mounts the
device. The device is unmounted by the **autounmountd** service after
the user navigates away and/or file copy operations stop.

For example, the above image shows USB drive "FreeNAS" attached to the
system. After double-clicking the desktop icon, "Insight File Manager"
opens to the device's location, autofs/da0. While Insight opens, the
automounter mounts the device. After closing Insight, the device is also
unmounted and safe to remove from the system.

In the CLI, the automounter adds a .desktop file to /media when a new
USB/Optical device is added. Open the .desktop file with xdg-open or
lumina-open. When the device is removed, the symlink is immediately
removed from /media.

> **note**
>
> The /.autofs/\* directories are not cleaned when the device is
> removed. However, after device removal the directories are no longer
> associated with the device in the backend. For this reason, /media is
> more useful to identify which devices are attached to the system.

Alternately, all device names are added to the /.autofs directory.
Attached devices are also accessed by navigating to
/.autofs/\<devicename\>.

Known limitations:

-   UFS permissions. These permissions are preserved on USB media. To
    allow multiple users access to files from a UFS stick, those files'
    permissions need to be set to *read/write by any user* (777).
-   ZFS pools are not yet supported. This is under investigation to
    ascertain if it can ever work with automount.
-   Optical Media links are not yet created on the desktop. Optical
    media is accessible by navigating to /.autofs.
-   Any file system with limited FreeBSD support (HFS or EXT) remain at
    the same level of limited support.
-   exFAT detection issues are being investigated.

Coming soon:

-   Optical media support for the desktop
-   Android device support
-   Possible support for ZFS pools

FreeBSD Ports
-------------

Trident allows users to build applications using the ports system. Use
git to fetch the FreeBSD ports tree on the local system. It is
recommended that users make use of the |Trident| branch of the FreeBSD
ports, which is regularly updated against the base FreeBSD ports tree.

> **note**
>
> These commands must be run as the superuser or **root**.

To fetch the ports for the first time, use the following command:

\# git clone http://github.com/Trident/freebsd-ports.git /usr/ports

To update an existing local ports directory:

``` {.sourceCode .none}
# cd /usr/ports
# git pull

The easiest way to find the location of the port you want to build is to browse the Trident ports on GitHub: https://github.com/Trident/freebsd-ports. From there, navigate to the directory of the required application and start build process.

For example, to build Filezilla:
```

``` {.sourceCode .none}
# cd /usr/ports/ftp/filezilla
# make install
```

Printing and Scanning
---------------------

Like many open source operating systems, |Trident| uses the Common Unix
Printing System ([CUPS](https://www.cups.org/)) to manage printing.

CUPS provides an easy-to-use utility for adding and managing printers.
Whether or not it automatically detects a printer depends upon how well
the printer is supported by an open source print driver. This section
walks you through a sample configuration for a HP DeskJet 36xx series
printer. Your specific printer may "just work", which simplifies this
process immensely. If your printer configuration does not work, read
this section more closely for ideas on locating correct drivers for your
printer.

### Researching your Printer

Before configuring your printer, see if a driver already exists for your
particular model, and if so, which driver is recommended. If you are
planning to purchase a printer, this is definitely good information to
know beforehand. Look up the vendor and model of the printer in the
[Open Printing Database](http://www.openprinting.org/printers), which
indicates if the model is supported and if there are any known caveats
with the print driver. Once the model is selected, click
Show this printer to see the results.

For the HP DeskJet model example, the HPLIP driver is recommended. In
|Trident|, the HPLIP driver is available as an optional package called
*hplip*. Use |appcafe| to search if the driver is installed, and install
it if not.

### Adding a Printer

Once printer support is determined, ensure the printer is plugged into
your computer or, if the printer is a network printer, both your
computer and the printer are connected to the network. Then, open a web
browser and enter the address 127.0.0.1:631/admin. This opens the CUPS
configuration, shown in Figure %s \<print4\>.

![Printer Configuration](images/print4a.png)

To add a new printer, click Add Printer. CUPS will pause for a few
seconds as it searches for available printers. When finished, a screen
similar to Figure %s \<print5\> is shown.

![Print Device Selection](images/print5a.png)

In this example, the wizard has found the HP DeskJet 3630 printer on
both the USB port (first entry) and the wireless network (second entry).
Click the desired connection method then click Continue. CUPS then
attempts to load the correct driver for the device. If successful, a
screen shown in Figure %s \<print6\> is shown.

![Describe Printer](images/print6a.png)

This screen automatically fills out the printer model series, a
description, and the type of connection. If desired, add a descriptive
Location. If sharing the printer on a network, check Sharing.

Once you click Continue, the next screen, shown in Figure %s \<print7\>,
displays a summary of the selected options and offers the ability to
select another driver. For now, leave the detected driver and click
Add Printer. If the printer does not work using the default driver, read
the Troubleshooting Printer Help section, which describes how to use
this screen in more detail.

![Viewing the Default Driver](images/print7a.png)

The next screen, shown in Figure %s \<print8\>, can be used to modify
the properties of the printer.

![Modify Print Properties](images/print8a.png)

It is recommended to take a few minutes to review the settings in the
General, Banners, and Policies tabs, as these allow configuration
options such as print banners, permissions, the default paper size, and
double-sided printing. The available settings can vary depending on the
capabilities of the print driver. When finished, click
Set Default Options to save the options. This opens the Printers tab
with the new printer displayed. An example is shown in
Figure %s \<print9\>.

![Manage Printer](images/print9a.png)

Print a test page to ensure the printer is working. Verify the printer
has paper and click Maintenance -\> Print Test Page. If a test page does
not print, refer to the Printer Help of this handbook.

### Manually Adding a Driver

If the print configuration fails, double-check the printer is supported
as described in Researching your Printer and HPLIP is installed if it is
a HP printer. Also check the printer is plugged in and powered on.

If the wizard is unable to even detect the device, try to manually add
the information for the print device. In the Select Device screen
(print5), select the type of connection to the printer and input all
necessary information. The type of information depends upon the type of
connection:

**USB:** This entry only appears if a printer is plugged into a USB port
and the number of entries vary depending on the number of USB ports on
the system. If there are multiple USB entries, highlight the one
representing the USB port your printer is plugged into.

**IPP:** Select this option if connecting to a printer cabled to another
computer (typically running a Microsoft operating system) sharing the
printer using IPP. Input the IP address of the printer and the name of
the print queue. To use IPP over an encrypted connection, select "ipps"
instead.

**HTTP:** This option allows you to manually type in the URI to the
printer. A list of possible URIs is available on the [CUPS
site](https://www.cups.org/doc/network.html). To use HTTP over an
encrypted connection, select https instead.

**AppSocket/HP JetDirect:** Select this option if connecting to an HP
network printer. Input the IP address of the printer. Only change the
port number if the printer is using a port other than the default of
*9100*.

**LPD/LPR:** Select this option if connecting to a printer which is
cabled to a Unix computer using LPD to share the printer. Input the
hostname and queue name of the Unix system.

After inputting the connection information, continue to add the printer
and test the connection by printing a test page as described in
Adding a Printer.

If the default driver is not working, try re-adding the printer. At the
print7 screen, try selecting a different driver.

Alternately, if you have a PPD driver from the manufacturer's website or
on the CD packed in with the printer, click Choose File to browse to the
location of the PPD file. PPD (PostScript Printer Description) is a
driver created by the manufacturer ending in a .ppd extension. Sometimes
the file ends with a .ppd.gz extension, indicating it is compressed.

### Scanning

While no scanning applications are included with |Trident|, there are a
few options available via |appcafe|. One good option is
[XSane](http://www.xsane.org/), a graphical utility for managing
scanners. The rest of this section describes using *XSane* for scanning.

To use your scanner, make sure the device is plugged into the |Trident|
system and click Browse Applications --\> Scanner or type xsane from the
command line. A pop-up message indicates XSane is detecting devices and
prompts you to accept the XSane license if a device is detected. If a
device is not detected, search for your device at the list of [supported
scanners](http://www.sane-project.org/sane-backends.html).

> **note**
>
> If the scanner is part of an HP All-in-One device, make sure the
> "hplip" package is installed. Use |appcafe| to see if the driver is
> installed, and install it if not.

Figure %s \<sane1\> shows the XSane interface running on a |Trident|
system attached to an HP DeskJet Printer/Scanner.

![XSane Interface](images/sane1.png)

The [XSane documentation](http://www.xsane.org/doc/sane-xsane-doc.html)
contains details on how to perform common tasks such as saving an image
to a file, photocopying an image, and creating a fax. It also describes
all of the icons in the interface and how to use them.

By default, XSane uses the default browser when clicking F1 to access
its built-in documentation. Configuring the default browser varies by
window manager so an Internet search may be necessary to set the default
browser setting.

Fonts
-----

|Trident| installs with [Google Noto](http://www.google.com/get/noto/)
which provides multi-lingual Sans and Serif fonts. Many other fonts are
available from |appcafe|. Typically, fonts installed using |appcafe| do
not require any additional configuration to "just work".

If you have downloaded or purchased a collection of font, |Trident| can
be configured to also use those fonts. Become the superuser and copy the
downloaded font to the /usr/local/share/fonts/ directory. Then, run
fc-cache -f -v /usr/local/share/fonts/name\_of\_font to refresh the
fonts cache.

Sound Mixer Tray
----------------

|Trident| includes a graphical utility for managing the sound card's
mixer settings. The utility is accessed by clicking the speaker icon in
the system tray.

Figure %s \<sound1\> shows an example of highlighting the Output option
after opening the Sound Mixer. If the system has one audio output, the
Output submenu is not displayed. Clicking an option in this submenu does
not set the default audio device. It only changes it to the selected
output for the current session. The next reboot reverts audio output
back to the default.

![Output Options](images/sound1.png)

Figure %s \<sound2\> shows the Mixer menu:

![Mixer Controls](images/sound2.png)

The Mixer Controls screen provides sliders to modify the left and right
channels that control volume, pcm (the sound driver), the speaker, the
microphone, the recording level, the input level, and the output level.
Each control can be muted or unmuted by clicking Mute or Unmute,
depending on its current mute state.

Figure %s \<sound3\> shows the System Configuration tab of the Mixer.

![System Sound Configuration](images/sound3a.png)

This tab contains several options:

-   **Recording Device:** Use the drop-down menu to select the device to
    use for recording sound.
-   **Default Tray Device:** Use the drop-down menu to set the default
    slider to display in the system tray.
-   **Audio Output Channel:** Use the drop-down menu to change the sound
    device and use Test to determine if sound is working. This is
    sometimes necessary when changing audio devices. For example, when
    connecting a USB headset, |Trident| detects the new device and
    automatically changes the audio device to the USB input. However,
    when inserting a headset into an audio jack, the system may not
    detect this new input, meaning the default device has changed
    manually. Set as Default sets the currently selected audio output
    channel as the system default.

The Disable PulseAudio disables all PulseAudio support.

The File menu can be used to quit this mixer screen or to close both
this screen and remove the icon from the system tray.

> **note**
>
> To re-add the mixer icon after removing it, type pc-mixer & in a
> command line. Alternately, open this application without adding it
> back to the system tray by typing pc-mixer -notray.

|Trident| provides full
[PulseAudio](https://www.freedesktop.org/wiki/Software/PulseAudio/)
support, which can be configured using the Configuration menu in the
Mixer. There are options for accessing the PulseAudio Mixer and
PulseAudio Settings, as well as an option for restarting PulseAudio.
These utilities can be used to configure discoverable network sound
devices and mixer levels.

Multimedia
----------

|Trident| is pre-configured to support most multimedia formats and makes
it easy to install most open source media applications using |appcafe|.

After installing a web browser, most media formats become playable,
including YouTube™ videos, Internet radio, and many trailer and movie
sites. When encountering a file unplayable in a web browser or media
player, it is likely in a proprietary format which requires a licensing
fee or restricts distribution of the codec required to play the media
format.

> **note**
>
> When troubleshooting Java™ or Flash for your browser, please refer to
> the [FreeBSD
> browser](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/desktop-browsers.html)
> documentation, which has more complete instructions for installing
> Java™ and Flash plugins with specific browsers.

|appcafe| contains several dozen applications for playing and editing
multimedia. It includes these popular applications:

-   [aTunes](http://www.atunes.org/?page_id=5): Full-featured audio
    player and manager which can play mp3, ogg, wma, wav, flac, mp4 and
    radio streaming, allowing users to easily edit tags, organize music
    and rip audio CDs.
-   [Audacity](https://sourceforge.net/projects/audacity/?lang=en):
    Multilingual audio editor and recorder.
-   [DeaDBeeF](http://deadbeef.sourceforge.net/screenshots.html): Music
    player supporting most audio formats.
-   [Decibel](http://decibel.silent-blade.org/index.php?n=Main.Screenshots):
    Audio player built around a highly modular structure which lets the
    user completely disable unneeded features. Able to play CDs
    directly.
-   [gtkpod](http://gtkpod.org/libgpod/): Graphical user interface for
    the Apple iPod.
-   [Miro](http://www.getmiro.com/download/screenshots/): HD video
    player which can play almost any video file and offers over 6,000
    free Internet TV shows and video podcasts.
-   [SMPlayer](http://www.smplayer.info/): Universal media player which
    can handle any media format and play audio CDs, DVDs, (S)VCDs,
    TV/radio cards, YouTube™ and SHOUTcast™ streams.
-   [VLC media player](http://www.videolan.org/vlc/): Open Source
    cross-platform multimedia player capable of playing most multimedia
    files, DVD and CD formats, and some streaming protocols.
-   [Kodi,formerly known as XBMC](https://kodi.tv/): GPL-licensed
    software media player and entertainment hub for digital media. It
    can play most audio and video formats, CDs and DVDs from a disk or
    image file, and even files inside ZIP and RAR archives.
-   [Plex Home Theater](https://www.plex.tv/): Centralized media
    playback system. The central Plex Media Server streams media to many
    Plex player Apps which are used to view your media library and watch
    shows.

Windows Emulation
-----------------

[Wine](https://wiki.winehq.org/Main_Page) is an application which allows
the creation of a Windows environment for installing Windows software.
This can be useful if your favorite Windows game or productivity
application has not yet been ported to Linux or BSD.

Wine is not guaranteed to work with every Windows application. You can
search for desired applications in the Browse Apps section of the [Wine
application database](https://appdb.winehq.org/). The [Wine
wiki](https://wiki.winehq.org/Main_Page) contains resources to get
started and troubleshooting reference material if problems are
encountered with a Windows application.

Wine can be installed using |appcafe|. After installing, it can be
started by typing winecfg in the command line. The first time running
this utility, it may prompt to install additional required packages. If
prompted, click Install in the pop-up menu.

The initial Wine configuration menu is shown in Figure %s \<wine1\>.

![Wine Configuration Menu](images/wine1a.png)

Click Add application to browse to the application's installer file. By
default, the contents of the hard drive will be listed under *drive\_c*.
If the installer is on a CD/DVD, use the drop-down menu to browse to the
home directory --\> \*.wine --\> dosdevices folder. The contents of the
CD/DVD should be listed under *d:*. If they are not, the most likely
reason is your CD/DVD was not automatically mounted by the desktop. To
mount the media, type mount -t cd9660 /dev/cd0 /cdrom as the superuser.

The system then accesses the media and you can now select the installer
file. Once selected, click Apply then OK to exit the configuration
utility.

To install the application, type winefile to see the screen shown in
Figure %s \<wine2\>.

![Installing the Application Using winefile](images/wine2a.png)

Click the button representing the drive which contains the installer and
double-click on the installation file (e.g. setup.exe). The installer
then launches to allow installing the application as on a Windows
system.

> **note**
>
> You may need need to unmount a CD/DVD before it ejects. As the
> superuser, type umount /mnt.

Once the installation is complete, browse to the application's location.
Figure %s \<wine3\> shows an example of running Internet Explorer within
winefile.

![Running the Installed Application](images/wine3.png)

Updating Trident
---------------

The Trident project is organized around two update tracks: STABLE and
UNSTABLE. Updating is handled through the |sysadm| Update Manager; refer
to the SysAdm Update Manager \<update-manager\> documentation for more
details about using the Update Manager. This section only contains
simple instructions to switch between update tracks.

To view or adjust the current update track for Trident, click
Start Menu --\> Control Panel --\> Update Manager --\> Settings. The
Settings tab, seen in Figure %s \<update1\>, allows you to adjust *when*
and *where* to perform system updates.

![Update Manager Settings](images/update1.png)

While both STABLE and UNSTABLE tracks are rolling releases based on
FreeBSD-CURRENT, there are a few key differences between them.

> **warning**
>
> Users with UNSTABLE installed before 7/14/2017 need to run
> pc-updatemanager syncconf in a command line in order to switch to the
> new UNSTABLE repository added on that day. Alternately, switch from
> UNSTABLE to STABLE in the Update Manager and click Save. Then, switch
> back to UNSTABLE and click Save again.

### Trident STABLE

As its name implies, STABLE refers to the more solid version of Trident.
STABLE updates are released infrequently, but are much more tested and
polished. All Trident installation files are created from the STABLE
track, and fresh Trident installations only look to the STABLE track for
updates.

The STABLE track is recommended for those users who want a more
predictable experience with fewer regressions, and are willing to wait
longer for bugfixes and new utilities or ports.

### Trident UNSTABLE

The UNSTABLE track is the bleeding edge of Trident development.
Experimental fixes, upstream patches from the FreeBSD project, and
testing new utilities and applications all happen first with the
UNSTABLE track.

UNSTABLE is recommended for power users, those with custom hardware
unsupported with STABLE, and project contributors who wish to help test
patches committed to Trident and/or FreeBSD-CURRENT.

To switch to the UNSTABLE track, open the SysAdm Update Manager and
navigate to the *Settings* tab, seen in update1. Check
UNSTABLE Repository, then click Save Settings.

Alternately, you can edit /usr/local/etc/Trident.conf to change update
tracks without using SysAdm. Here is an example Trident.conf:

``` {.sourceCode .none}
# Trident Configuration Defaults

# Default package set to pull updates from
PACKAGE_SET: <STABLE, UNSTABLE, or CUSTOM>
PACKAGE_URL: <CUSTOM url>

# Default type of CDN to use
# IPFS - Use IPFS
# HTTP - Use a standard HTTP connection (default)
# CDN_TYPE: HTTP

# Set the number of automatic boot-environments to create / keep
MAXBE: 5
AUTO_UPDATE: disabled
AUTO_UPDATE_REBOOT: disabled
```

Rolling back from UNSTABLE to STABLE is done by switching tracks back to
the STABLE branch, checking for updates, and rebooting once the previous
STABLE update is installed.
