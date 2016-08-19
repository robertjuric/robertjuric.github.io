---
author: admin
comments: true
date: 2015-09-20 16:23:09+00:00
layout: post
slug: building-xen-lvm-domu
title: Building Xen LVM DomU
wordpress_id: 1235
categories:
- Virtualization
tags:
- Debian
- Debian 8
- DomU
- Guest
- LVM
- Xen
---

Building a Xen DomU (guest) using LVM storage and Debian as a guest OS.

If VNC viewer is not installed, you can install gnvcviewer
`apt-get install gvncviewer`

Create LVM logical volume, Syntax is: lvcreate -n -L<size, you can use G and M here>
`lvcreate -nguest-os -L10G vg0`

Created guest config file
`nano guest.cfg`

    
    <code>kernel = "/usr/lib/xen-4.4/boot/hvmloader"
    builder='hvm'
    memory = 4096
    vcpus = 2
    name = "plex"
    vif = ['bridge=xenbr0']
    disk = ['phy:/dev/vg0/guest-os,hda,w','file:<filepath>.iso,hdc:cdrom,r']
    acpi = 1
    device_model_version = 'qemu-xen'
    boot="d"
    sdl=0
    serial='pty'
    vnc=1
    vnclisten=""
    vncpasswd=""</code>



Start guest
`xl create guest.config`

View GUI
`gvncviewer localhost`

Destroy Guest
`xl destroy guest`

Open console using VNC Viewer, Complete Install Guest OS from the .iso file
Destroy Guest
Change boot="d" to boot="c" in the config file
Start guest
