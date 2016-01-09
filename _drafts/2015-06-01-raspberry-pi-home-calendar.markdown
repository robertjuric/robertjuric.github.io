---
author: admin
comments: true
date: 2015-06-01 16:07:37+00:00
layout: post
slug: raspberry-pi-home-calendar
title: Raspberry Pi Home Calendar
wordpress_id: 1184
categories:
- General Tech
tags:
- calendar
- calendar display
- fullscreen
- mpanel
- pi
- rapberry pi
---

I bought a Raspberry Pi a while back with grand delusions of a digital signage/home calendar display for our kitchen. After coming up short on solutions I eventually gave up and boxed it all back up. Well I stumbled across a solution the other day that gave me reason to dig the Pi back out and get that display working.


## The Display Solution


The solution I finally found that warranted display in our kitchen was [mPanel](http://designelemental.net/mpanel/). mPanel allows integration with iCal links, Weather Underground API for custom weather, links to Flickr for beautiful background photos, and does all of this in a beautiful package. Setting up mPanel is fairly straightforward. You'll need to request a free API from Weather Underground for local weather, the display looks amazing and even has red alert boxes in case of storm warnings. I linked it to my Google iCal URL to get a feed of calendar events, however I noticed that if there is an alert configured it would hide the event name, this is probably something I'll circle back to later. I also created a Flickr album with photos around our backyard to cycle through as backgrounds.  Once I had mPanel all configured, it was a simple matter of making it display properly on the Pi.

[![home display](http://robertjuric.com/wp-content/uploads/2015/06/home-display-e1433174693577-300x186.jpg)](http://robertjuric.com/wp-content/uploads/2015/06/home-display.jpg)


## Full Resolution Display


I already had my Pi running previously on a [NOOBS/Raspbian](https://www.raspberrypi.org/downloads/) image. The first thing I had to fix was the HDMI output on my TV had black bars on the sides and wasn't utilizing true full screen. This was a simple fix by disabling overscan.

1. From CLI:
`sudo nano /boot/config.txt`
2. Remove top comment so that:
`disable_overscan=1`
3. Update NOOBS config at bottom so that:
`#overscan_left=24
#overscan_right=24
#overscan_top=16
#overscan_bottom=16
disable_overscan=1`
4. Ctrl+X to save/exit. Y to save.
5. `sudo reboot`

I was then able to start the GUI full screen open Midori and view my mPanel page in all its glory. Except that it would occasionally hang and not refresh. I realized that the full GUI was pegging CPU utilization at 100%, so I thought it would be better if I could launch Midori directly from the CLI. After some research I finally found a solution that was lightweight enough.

The first thing I had to do was install the [Matchbox window manager,](https://www.yoctoproject.org/tools-resources/projects/matchbox) x11-xserver utilities to keep the display on 24/7, and unclutter to hide the mouse:
`sudo apt-get install matchbox
sudo apt-get install x11-xserver-utils
sudo apt-get install unclutter`
Then I created a startup script
`sudo nano startMidori`
Which looked like:
`#!/bin/sh
xset -dpms  # disable DPMS (EnergyStar) features
xset s off  # disable screen saver
xset s noblank  # don't blank the video service
sudo unclutter &
matchbox-window-manager &
midori -e Fullscreen -a http://startupsite.com`
Ctrl+X to save/exit. Y to save.
I was then able to run the script to open Midori full screen to my mPanel page:
`xinit ./startMidori`


## Automate It


Now that I had a working script and a working display I just needed a way to automate the login process and auto-execute my script. The first thing we have to do is automate the login process. To do this we need to make some changes to the /etc/inittab file.

Open the file:
`sudo nano /etc/inittab`
Navigate in the file to the following line:
`1:2345:respawn:/sbin/getty 115200 tty1`
Put a comment before that line to disable getty:
`#1:2345:respawn:/sbin/getty 115200 tty1`
Right after that line add the following line to run the login program with Pi user:
`1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2>&1`
Ctrl+X to save/exit. Y to save.

Now to just run my Midori script after login we will edit the profile:
`sudo nano /etc/profile`
Add the following line to the end of the file:
`xinit ./startMidori`
Ctrl+X to save/exit. Y to save.

Now just reboot the Pi one more time (sudo reboot) and enjoy your automated calendar goodness.

Here is my Pi until I get around to purchasing a case and hiding it behind the TV:

[![IMG_2816](http://robertjuric.com/wp-content/uploads/2015/06/IMG_2816-225x300.jpg)](http://robertjuric.com/wp-content/uploads/2015/06/IMG_2816.jpg)
