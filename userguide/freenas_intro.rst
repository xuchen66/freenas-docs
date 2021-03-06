.. centered:: FreeNAS® is © 2011-2016 iXsystems

.. centered:: FreeNAS® and the FreeNAS® logo are registered trademarks
              of iXsystems.

.. centered:: FreeBSD® is a registered trademark of the FreeBSD
              Foundation

Written by users of the FreeNAS® network-attached storage operating
system.

Version |release|

Copyright © 2011-2016
`iXsystems <https://www.ixsystems.com/>`_

This Guide covers the installation and use of FreeNAS® |release|.

The FreeNAS® Users Guide is a work in progress and relies on the
contributions of many individuals. If you are interested in helping us
to improve the Guide, read the instructions in the `README
<https://github.com/freenas/freenas/blob/master/docs/userguide/README.md>`_.
IRC Freenode users are welcome to join the *#freenas* channel
where you will find other FreeNAS® users.

The FreeNAS® Users Guide is freely available for sharing and
redistribution under the terms of the `Creative Commons Attribution
License <https://creativecommons.org/licenses/by/3.0/>`_. This means
that you have permission to copy, distribute, translate, and adapt the
work as long as you attribute iXsystems as the original source of the
Guide.

FreeNAS® and the FreeNAS® logo are registered trademarks of iXsystems.

Active Directory® is a registered trademark or trademark of Microsoft
Corporation in the United States and/or other countries.

Apple, Mac and Mac OS are trademarks of Apple Inc., registered in the
U.S. and other countries.

Chelsio® is a registered trademark of Chelsio Communications.

Cisco® is a registered trademark or trademark of Cisco Systems, Inc.
and/or its affiliates in the United States and certain other
countries.

Django® is a registered trademark of Django Software Foundation.

Facebook® is a registered trademark of Facebook Inc.

FreeBSD and the FreeBSD logo are registered trademarks of the FreeBSD
Foundation.

Fusion-io is a trademark or registered trademark of Fusion-io, Inc.

Intel, the Intel logo, Pentium Inside, and Pentium are trademarks of
Intel Corporation in the U.S. and/or other countries.

LinkedIn® is a registered trademark of LinkedIn Corporation.

Linux® is a registered trademark of Linus Torvalds.

Marvell® is a registered trademark of Marvell or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates.

Twitter is a trademark of Twitter, Inc. in the United States and other
countries.

UNIX® is a registered trademark of The Open Group.

VirtualBox® is a registered trademark of Oracle.

VMware® is a registered trademark of VMware, Inc.

Wikipedia® is a registered trademark of the Wikimedia Foundation,
Inc., a non-profit organization.

Windows® is a registered trademark of Microsoft Corporation in the
United States and other countries.

**Typographic Conventions**

The FreeNAS® |release| Users Guide uses the following typographic
conventions:

* Names of graphical elements such as buttons, icons, fields, columns,
  and boxes are enclosed within quotes. For example: click the "Import
  CA" button.

* Menu selections are italicized and separated by arrows. For example:
  :menuselection:`System --> Information`.

* Commands that are mentioned within text are highlighted in
  :command:`bold text`. Command examples and command output are
  contained in green code blocks.

* Volume, dataset, and file names are enclosed in a blue box
  :file:`/like/this`.

* Keystrokes are formatted in a blue box. For example: press
  :kbd:`Enter`.

* **bold text:** used to emphasize an important point.

* *italic text:* used to represent device names or text that is input
  into a GUI field.

.. _Introduction:

Introduction
============

FreeNAS® is an embedded open source network-attached storage (NAS)
operating system based on FreeBSD and released under a
`2-clause BSD license <https://opensource.org/licenses/BSD-2-Clause>`_.
A NAS has an operating system optimized for file storage and sharing.

FreeNAS® provides a browser-based, graphical configuration interface.
The built-in networking protocols provide storage access to multiple
operating systems. A plugin system is provided for extending the
built-in features by installing additional software.

.. _What's New in |release|:

What's New in |version|
-----------------------

FreeNAS® uses a "rolling release" model instead of point releases. The
:ref:`Update` mechanism makes it easy to keep up-to-date with the
latest security fixes, bug fixes, and new features. Some updates
affect the user interface, so this section lists any functional
changes that have occurred since |version| was released.

.. note:: The screenshots in this documentation assume that your
   system is fully updated to the latest STABLE version of FreeNAS®
   |version|. If a screen on your system looks different than the
   documentation, make sure that the system is fully up-to-date. If is
   is not, apply any outstanding updates.

* Samba has been updated to
  `4.3.11 <https://www.samba.org/samba/history/samba-4.3.11.html>`_.

* The "Update Server" URL in
  :menuselection:`System --> Update`
  has changed to *update.ixsystems.com*. Due to the way the updater
  works, it will take two update cycles for existing FreeNAS®
  installations to reflect this change.

* When security certificates or SSH keys are generated, the
  fingerprints are logged in :file:`/var/log/messages`,
  :file:`var/log/debug.log`, and the console.

* Dashes have been added to the characters allowed in jail names.

* The GUI prevents duplicate MAC addresses from being entered for
  jails.

* A new alert is shown when attempting to bind NFS to a non-existent
  IP address.

.. index:: Hardware Recommendations
.. _Hardware Recommendations:

Hardware Recommendations
------------------------

Since FreeNAS® |release| is based on FreeBSD 10.3, it supports the
same hardware found in the
`FreeBSD Hardware Compatibility List
<http://www.freebsd.org/releases/10.3R/hardware.html>`__.
Supported processors are listed in section
`2.1 amd64
<https://www.freebsd.org/releases/10.3R/hardware.html#proc>`_.
FreeNAS® is only available for 64-bit (also known as amd64)
processors.

.. note:: FreeNAS® boots from a GPT partition. This means that the
   system BIOS must be able to boot using either the legacy BIOS
   firmware interface or EFI.

Actual hardware requirements vary depending on the usage of the
FreeNAS® system. This section provides some starter guidelines. See
the
`FreeNAS® Hardware
Forum <https://forums.freenas.org/index.php?forums/hardware.18/>`_
for performance tips from other FreeNAS® users or to post questions
regarding the hardware best suited to meet your requirements. This
`forum post
<https://forums.freenas.org/index.php?threads/hardware-recommendations-read-this-first.23069/>`__
provides some specific recommendations for those planning on
purchasing hardware. Refer to
`Building, Burn-In, and Testing your FreeNAS system
<https://forums.freenas.org/index.php?threads/building-burn-in-and-testing-your-freenas-system.17750/>`_
for detailed instructions on how to test new hardware.

.. _RAM:

RAM
~~~

The best way to get the most out of a FreeNAS® system is to install
as much RAM as possible. The recommended minimum is 8 GB of RAM. The
more RAM, the better the performance, and the
`FreeNAS® Forums <https://forums.freenas.org/index.php>`_
provide anecdotal evidence from users on how much performance is
gained by adding more RAM.

Depending upon the use case, your system may require more RAM. Here
are some general rules of thumb:

* To use ZFS deduplication, ensure the system has at least 5 GB of RAM
  per TB of storage to be deduplicated.

* To use Active Directory with many users, add an additional 2 GB of
  RAM for winbind's internal cache.

* When :ref:`Using the phpVirtualBox Template`, increase the minimum
  RAM size by the amount of virtual memory configured for use in
  virtual machines. For example, if there will be two virtual
  machines, each with 4 GB of virtual memory, the system needs at
  least 16 GB of RAM.

* To use iSCSI, install at least 16 GB of RAM if performance is not
  critical, or at least 32 GB of RAM if good performance is a
  requirement.

* When installing FreeNAS® on a headless system, disable the shared
  memory settings for the video card in the BIOS.

If the hardware supports it and the budget allows for it, install ECC
RAM. While more expensive, ECC RAM is highly recommended as it
prevents in-flight corruption of data before the error-correcting
properties of ZFS come into play, thus providing consistency for the
checksumming and parity calculations performed by ZFS. If you consider
your data important, use ECC RAM. This
`Case Study
<http://research.cs.wisc.edu/adsl/Publications/zfs-corruption-fast10.pdf>`_
describes the risks associated with memory corruption.

Unless the system has at least 8 GB of RAM, consider adding RAM before
using FreeNAS® to store data. Many users expect FreeNAS® to function
with less memory, just at reduced performance.  The bottom line is
that these minimums are based on feedback from many users. Requests
for help in the forums or IRC are sometimes ignored when the installed
system does not have at least 8 GB of RAM because of the abundance of
information that FreeNAS® may not behave properly with less memory.

.. _Compact or USB Flash:

Compact or USB Flash
~~~~~~~~~~~~~~~~~~~~

The FreeNAS® operating system is installed to at least one device that
is separate from the storage disks. The device can be a USB stick,
compact flash, or SSD. Technically, it can also be installed onto a
hard drive, but this is discouraged as that drive will then become
unavailable for data storage.

.. note:: to write the installation file to a USB stick, **two** USB
   ports are needed, each with an inserted USB device. One USB stick
   contains the installer.  The other USB stick is the destination for
   the FreeNAS® installation. Take care to select the correct USB
   device for the FreeNAS® installation. It is **not** possible to
   install FreeNAS® onto the same USB stick containing the installer.
   After installation, remove the installer USB stick. It might also
   be necessary to adjust the BIOS configuration to boot from the new
   FreeNAS® USB stick.

When determining the type and size of the target device where FreeNAS®
will be installed, keep these points in mind:

- the *bare minimum* size is 8 GB. This provides room for the
  operating system and several boot environments. Since each update
  creates a boot environment, this is the *recommended* minimum. 16 GB
  provides room for more boot environments.

- if you plan to make your own boot environments, budget about 1 GB of
  storage per boot environment. Consider deleting older boot
  environments after making sure they are no longer needed. Boot
  environments can be created and deleted using
  :menuselection:`System --> Boot`.

- when using a USB stick, it is recommended to use a quality, name
  brand USB stick as ZFS will quickly reveal errors on cheap, poorly
  made sticks.

- for a more reliable boot disk, use two identical devices and select
  them both during the installation. This will create a mirrored boot
  device.

.. _Storage Disks and Controllers:

Storage Disks and Controllers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `Disk section
<http://www.freebsd.org/releases/10.3R/hardware.html#DISK>`_
of the FreeBSD Hardware List lists the supported disk controllers. In
addition, support for 3ware 6 Gbps RAID controllers has been added
along with the CLI utility :command:`tw_cli` for managing 3ware RAID
controllers.

FreeNAS® supports hot pluggable drives. Using this feature requires
enabling AHCI in the BIOS.

Reliable disk alerting and immediate reporting of a failed drive can
be obtained by using an HBA such as an Avago MegaRAID controller or a
3Ware twa-compatible controller.

Suggestions for testing disks before adding them to a RAID array can
be found in this
`forum post
<https://forums.freenas.org/index.php?threads/checking-new-hdds-in-raid.12082/>`__.

`This article <http://technutz.com/purpose-built-nas-hard-drives/>`_
provides a good overview of hard drives which are well suited for a
NAS.

If the budget allows optimization of the disk subsystem, consider the
read/write needs and RAID requirements:

* For steady, non-contiguous writes, use disks with low seek times.
  Examples are 10K or 15K SAS drives which cost about $1/GB. An
  example configuration would be six 600 GB 15K SAS drives in a RAID
  10 which would yield 1.8 TB of usable space, or eight 600 GB 15K SAS
  drives in a RAID 10 which would yield 2.4 TB of usable space.

* 7200 RPM SATA disks are designed for single-user sequential I/O and
  are not a good choice for multi-user writes.

When high performance is a key requirement and budget permits,
consider a
`Fusion-I/O card <http://www.fusionio.com/products/>`_
which is optimized for massive random access. These cards are
expensive and are suited for high-end systems that demand performance.
A Fusion-I/O card can be formatted with a filesystem and used as
direct storage; when used this way, it does not have the write issues
typically associated with a flash device. A Fusion-I/O card can also
be used as a cache device when your ZFS dataset size is bigger than
your RAM. Due to the increased throughput, systems running these cards
typically use multiple 10 GigE network interfaces.

For ZFS,
`Disk Space Requirements for ZFS Storage Pools
<http://docs.oracle.com/cd/E19253-01/819-5461/6n7ht6r12/index.html>`_
recommends a minimum of 16 GB of disk space. Due to the way that ZFS
creates swap,
**it is not possible to format less than 3 GB of space with ZFS**.
However, on a drive that is below the minimum recommended size, a fair
amount of storage space is lost to swap: for example, on a 4 GB
drive, 2 GB will be reserved for swap.

Users new to ZFS who are purchasing hardware should read through
`ZFS Storage Pools Recommendations
<http://www.solarisinternals.com/wiki/index.php/ZFS_Best_Practices_Guide#ZFS_Storage_Pools_Recommendations>`_
first.

ZFS *vdevs*, groups of disks that act like a single device, can be
created using disks of different sizes.  However, the capacity
available on each disk is limited to the same capacity as the smallest
disk in the group. For example, a *vdev* with one 2 TB and two 4 TB
disks will only be able to use 2 TB of space on each disk. In general,
use disks that are the same size for the best space usage and
performance.

.. _Network Interfaces:

Network Interfaces
~~~~~~~~~~~~~~~~~~

The `Ethernet section
<http://www.freebsd.org/releases/10.3R/hardware.html#ethernet>`_
of the FreeBSD Hardware Notes indicates which interfaces are supported
by each driver. While many interfaces are supported, FreeNAS® users
have seen the best performance from Intel and Chelsio interfaces, so
consider these brands when purchasing a new NIC. Realtek cards often
perform poorly under CPU load as interfaces with these chipsets do not
provide their own processors.

At a minimum, a GigE interface is recommended. While GigE interfaces
and switches are affordable for home use, modern disks can easily
saturate their 110 MB/s throughput. For higher network throughput,
multiple GigE cards can be bonded together using the LACP type of
:ref:`Link Aggregations`. The Ethernet switch must support LACP, which
means a more expensive managed switch is required.

When network performance is a requirement and there is some money to
spend, use 10 GigE interfaces and a managed switch. Managed switches
with support for LACP and jumbo frames are preferred, as both can be
used to increase network throughput. Refer to the
`10 Gig Networking Primer
<https://forums.freenas.org/index.php?threads/10-gig-networking-primer.25749/>`_
for more information.

.. note:: At present, these are not supported: InfiniBand,
   FibreChannel over Ethernet, or wireless interfaces.

Both hardware and the type of shares can affect network performance.
On the same hardware, CIFS is slower than FTP or NFS because Samba is
`single-threaded
<https://www.samba.org/samba/docs/man/Samba-Developers-Guide/architecture.html>`_.
So a fast CPU can help with CIFS performance.

Wake on LAN (WOL) support depends on the FreeBSD driver for the
interface. If the driver supports WOL, it can be enabled using
`ifconfig(8) <http://www.freebsd.org/cgi/man.cgi?query=ifconfig>`_. To
determine if WOL is supported on a particular interface, use the
interface name with the following command. In this example, the
capabilities line indicates that WOL is supported for the *re0*
interface::

 ifconfig -m re0
 re0: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> metric 0 mtu 1500
 options=42098<VLAN_MTU,VLAN_HWTAGGING,VLAN_HWCSUM,WOL_MAGIC,VLAN_HWTSO>
 capabilities=5399b<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,VLAN_HWCSUM,TSO4,WOL_UCAST,WOL_MCAST, WOL_MAGIC,VLAN_HWFILTER,VLAN_H WTSO>

If you find that WOL support is indicated but not working for a
particular interface, create a bug report using the instructions in
:ref:`Support`.

.. include:: zfsprimer.rst
