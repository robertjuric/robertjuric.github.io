---
author: admin
comments: true
date: 2015-12-18 17:06:58+00:00
layout: post
slug: esxi-bootable-usb-from-mac-osx
title: ESXi Bootable USB from Mac OSX
wordpress_id: 1289
categories:
- Virtualization
tags:
- bootable
- el capitan
- esxi
- mac
- osx
- usb
---

I found a great fix to be able to create use the ESXi installer .iso and copy it to a bootable USB. Neither my MBA nor my physical host has an optical drive. I found the solution on the [Mac Repairs Lond blog](http://blog.macrepairsouthlondon.com/hardware/networking/create-esxi-usb-installer-mac-os-x/) but I wanted to summarize for posterity:

Format the USB drive using DiskUtility with FAT and a MBR.
Open the terminal and find the mount point of the USB drive
`diskutil list`
Then unmount the disk and use fdisk to mark the partition as bootable
`diskutil unmountdisk /dev/disk#`
`fdisk -e /dev/disk#`
This enters fdisk interactive mode so enter the following commands to mark the partition and write the changes.

    
    <code>f 1
    write
    quit</code>


At this point I mounted the USB drive and copied all the contents from the .iso to the USB drive.
One last step, find the file `ISOLINUX.CFG` and rename it to `SYSLINUX.cfg`. Then edit the file and find the line that starts `APPEND -c boot.cfg` and append `-p 1` to the end. Save and Exit. You're ready to stick the USB drive in and boot the ESXi installer from USB.
