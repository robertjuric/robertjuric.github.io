---
author: admin
comments: false
date: 2015-09-20 14:48:23+00:00
layout: post
slug: debian-8-xen-dom0
title: Debian 8 Xen Dom0
wordpress_id: 1212
categories:
- Virtualization
tags:
- Debian
- Debian 8
- Dom0
- Xen
---

Below are my living notes of how I installed Debian 8 as a Xen Dom0 (host). I noticed some of the commands had been updated in this version and my system uses SATA drives and the onboard ethernet NIC so some commands reference "sda" and "eno1" instead of hda and eth1.


## Debian Install


Boot install media, I used the network install ISO I burned to a CD.
Select language, host name, timezone and other settings
At Partition Disks, Select Manual and Continue.


I decided to go head and build 4 partitions from the beginning. If you're using a separate drive for storing the domU/guest drives then you could just use the 3. Or if you're more experienced you could use whichever layout you prefer.


Select SCSI1 (sda) or disk you're installing OS to and continue
Select yes if asked to create a new partition table
sda1 - Partition


Select the pri/log FREE SPACE and Continue
Create a new partition and Continue
New partition size: 200 MB and Continue
Select Primary and Continue
Select Beginning and Continue
Double-click Use as: and set to Ext2
Double-click Mount point and select '/boot - the static files of the boot loader'
Double-click Bootable flag to set to on
Select Done setting up the partition and Continue


sda2 -Root Partition


Select the pri/log FREE SPACE and Continue
Create a new partition and Continue
New partition size: 4 GB and Continue
Select Primary and Continue
Select Beginning and Continue
Verify Use as: Ext4 journaling file system
Double-click Mount point and select '/ - the root file system'
Select Done setting up the partition and Continue


sda3 -Swap Partition


Select the pri/log FREE SPACE and Continue
Create a new partition and Continue
New partition size: 2 GB and Continue
Select Primary and Continue
Select Beginning and Continue
Double-click Use as and select 'swap area'
Select Done setting up the partition and Continue


sda4 -DomU Storage


Select the pri/log FREE SPACE and Continue
Create a new partition and Continue
Accept default size (remaining disk) and Continue
Select Primary and Continue
Select Beginning and Continue
Double-click Use as and select 'physical volume for LVM'
Select Done setting up the partition and Continue
Select finish partitioning and write changes to disk (at bottom) and Continue
Select Yes to write the changes to disks and Continue


Continue Installing OS as Normal
After boot, login.


## Xen Setup


Install Xen
`apt-get install xen-linux-system`
Prioritize Booting Xen Over Native
`dpkg-divert --divert /etc/grub.d/08_linux_xen --rename /etc/grub.d/20_linux_xen`
`update-grub`
Reboot.


## Storage Setup


Install LVM
`apt-get install lvm2`
Configure LVM to use /dev/sda4 as its physical volume, or whichever volume you decide to use for domU storage
`pvcreate /dev/sda4`
Create a volume group on the physical volume
`vgcreate vg0 /dev/sda4`
(Now LVM is setup and initialized and able to create logical volumes)
``


## Networking


Set DNS Servers
`nano /etc/resolv.config`

    
    <code>"nameserver 8.8.8.8"
    "nameserver 8.8.4.4"</code>


Configure network bridging
`nano /etc/network/interfaces`

    
    <code>auto lo
    iface lo inet loopback
    
    auto eno1
    iface eno1 inet manual
    
    auto xenbr0
    iface xenbr0 inet static
    address 192.168.1.56
    netmask 255.255.255.0
    gateway 192.168.1.1
    bridge_ports eno1</code>


Reboot.

I'll continue my Xen configuration in another post.
