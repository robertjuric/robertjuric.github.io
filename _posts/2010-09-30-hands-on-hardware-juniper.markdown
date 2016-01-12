---
author: robertjuric
comments: true
date: 2010-09-30 01:07:42+00:00
layout: post
slug: hands-on-hardware-juniper
title: Hands on Hardware - Juniper
wordpress_id: 226
categories:
- Insights
- Networking
tags:
- EX200
- EX4200
- hardware
- Juniper
- SRX650
---


So we've recently decided to consolidate our two existing data centers into a single data center and move them to an off-site co-location. Part of this new project involved redesigning our data center network. Let me tell you, know is an exciting time to be building a new data center. We looked at Cisco, Juniper, and Brocade to get their proposals for our new data center. Brocade at the time did not have a 10GE top-of-rack switch. Cisco, with their Nexus line looked like a solid option, though the missing L3 functionality in the 5020 pushed us away. In the end, Juniper had a solid offering at a competitive price. There was some hesitation moving away from an all Cisco network, though in the end we are pleased with our decision.

Our design consists of SRX650s for our edge, EX4500s in the top-of-rack, and also EX4500s for our core. We debated what to put in the core, but with our small footprint (we're looking at two racks once we are more virtualized) we decided that the EX4500s would suffice. There will also be some 4200s and 2200s for some other peripheral uses.

Well we cut POs, and the blue boxes have started to show up. I thought I'd share some pictures I took while I was inventorying and assembling the devices and modules. 

The new EX2200:
<table style="width:auto;" ><tr >
<td >[![](http://lh6.ggpht.com/_uYKdbaxxVC0/TKPXUC4OcjI/AAAAAAAAAfM/aFRbSQIkJNk/s144/EX2200.jpg)](http://picasaweb.google.com/lh/photo/3JUSbSajmnZZ8LjR-qUOiw?feat=embedwebsite)
</td></tr><tr >
<td style="font-family:arial,sans-serif;font-size:11px;text-align:right;" >From [Data Center](http://picasaweb.google.com/robert.juric/DataCenter?feat=embedwebsite)
</td></tr></table>

Installing an uplink module in the EX4200:
<table style="width:auto;" ><tr >
<td >[![](http://lh6.ggpht.com/_uYKdbaxxVC0/TKPYmcAx6pI/AAAAAAAAAfo/8_pYCLJ9yA4/s144/EX4200-Front-Reassembly.jpg)](http://picasaweb.google.com/lh/photo/JBH8V9SxFkLV34fHHfWeWQ?feat=embedwebsite)
</td></tr><tr >
<td style="font-family:arial,sans-serif;font-size:11px;text-align:right;" >From [Data Center](http://picasaweb.google.com/robert.juric/DataCenter?feat=embedwebsite)
</td></tr></table>

The SRX650 with 24port switch module installed:
<table style="width:auto;" ><tr >
<td >[![](http://lh4.ggpht.com/_uYKdbaxxVC0/TKPZK7EhQmI/AAAAAAAAAgE/83mUfG5KnkE/s144/SRX650-Front-Modules.jpg)](http://picasaweb.google.com/lh/photo/0FKxsfidrwynnKJd4mVx1w?feat=embedwebsite)
</td></tr><tr >
<td style="font-family:arial,sans-serif;font-size:11px;text-align:right;" >From [Data Center](http://picasaweb.google.com/robert.juric/DataCenter?feat=embedwebsite)
</td></tr></table>

The gear for one of our companies: There is a similar stack for the 2nd company we are splitting out.
<table style="width:auto;" ><tr >
<td >[![](http://lh3.ggpht.com/_uYKdbaxxVC0/TKPYm1QUTlI/AAAAAAAAAfw/pQujNz6Phrw/s144/JuniperGear1.jpg)](http://picasaweb.google.com/lh/photo/D04OD5gEC69qkpIyt-E_pw?feat=embedwebsite)
</td></tr><tr >
<td style="font-family:arial,sans-serif;font-size:11px;text-align:right;" >From [Data Center](http://picasaweb.google.com/robert.juric/DataCenter?feat=embedwebsite)
</td></tr></table>

Unfortunately the much anticipated EX4500s have not arrived yet, but as soon as then do I'll post some more hand-on pics.
