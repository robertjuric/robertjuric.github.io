---
author: admin
comments: true
date: 2015-12-18 17:18:28+00:00
layout: post
slug: installing-cisco-virl-using-esxi-host-client
title: Installing Cisco VIRL using ESXi Host Client
wordpress_id: 1276
categories: ['networking']
tags:
- Cisco
- esxi
- esxi host client
- host client
- install
- setup
- virl
---

I recently gave away my last Windows PC and vowed never to return. As a result I'm left with a MacBook Air for our general home use. I've wanted to get a VIRL lab setup since I heard about it, I even purchased some modest hardware to use as a host. However I was hesitant to proceed with VIRL because I was concerned with being able to manage my lone ESXi host without a Windows PC to install the vSphere client. I recently learned about a great new tool to manage standalone ESXi servers, the [ESXi Embedded Host Client](https://labs.vmware.com/flings/esxi-embedded-host-client).

The host client is a VIB that you install directly on your ESXi host which offers a web interface to manage the host. You're able to configure most items on the host, access VM consoles, and some limited performance monitoring. It really is a great tool, however it doesn't have the full functionality that the vSphere client does. It is also only a Fling with VMware Labs, so its future is not certain. With this new tool I decided to plow along and get my VIRL lab up and running.

The first first issue I ran into was getting the ESXi .iso file onto a boot-able USB drive since my host hardware did not have an optical drive (neither does my MBA). The DiskUtility on OSX was recently changed with El Capitan so most of the how-to articles I found didn't apply or wouldn't fully function. I finally found a working solution from the [Mac Repairs London blog](http://blog.macrepairsouthlondon.com/hardware/networking/create-esxi-usb-installer-mac-os-x/). I [summarized it here](http://robertjuric.com/tech/esxi-bootable-usb-from-mac-osx/) for posterity.

To get started I first installed ESXi (I used version 6 Update1). After the installation was completed I configured a static IP and enabled SSH. I then proceeded to install the ESXi Host Client VIB. The instructions are on the Host Client website. I used the SCP + SSH options to copy the file over and then execute the esxcli commands to install it. Now I'm able to view a web interface for the host.

The next step was to proceed with the VIRL installation. When purchasing my license I selected the VM option. The [VIRL website](http://virl-dev-innovate.cisco.com/) has instructions for importing the OVA file using either the vSphere Client or the vSphere Web Client. I was able to follow the instructions for the vSphere client with only a few slight modifications:

Using the ESXi Host Client I was able to build the required port-groups (Flat, Flat1, SNAT, INT), however I had no options to enable promiscuous mode. I dug around the esxcli documentation and found out how to set this via ssh. I used SSH to connect to the host and issued the following commands (after creating the port-groups in the Host Client).

    
    <code>esxcli network vswitch standard portgroup policy security set -o true -p Flat
    esxcli network vswitch standard portgroup policy security set -o true -p Flat1
    esxcli network vswitch standard portgroup policy security set -o true -p SNAT
    esxcli network vswitch standard portgroup policy security set -o true -p INT</code>


After doing so command `esxcli network vswitch standard portgroup policy security get -p Flat` or the Host Client will display the new, correct settings. 

I then proceeded to import the .ova file per the VIRL instructions. However, after a few failed attempts I finally noticed a message on the Host Client stating that if you're attempting to import an .OVA over 1gb, you should extract and import the individual .ovf and .vmdk files. So on my OSX terminal I issued the command `tar -zxvf virl.ova` and then repeated the process but instead of selecting the .ova file I selected the individually extracted files and the import job successfully completed.

Now I've got an ESXi host and a VIRL lab all of which I can manage from my MBA!
