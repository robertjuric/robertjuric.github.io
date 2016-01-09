---
author: robertjuric
comments: true
date: 2010-05-24 02:14:58+00:00
layout: post
slug: whiteboard-arp-process
title: Whiteboard - ARP Process
wordpress_id: 98
categories:
- Insights
- Networking
---

I felt I would start off with something simple for my own sake. Below is  the ARP process where PC1 is attempting to communicate to the IP  address of PC2.




![](http://robertj.files.wordpress.com/2010/05/100_06401.jpg?w=1024)


I felt it is also important to view the contents of an ARP packet:

![](http://robertj.files.wordpress.com/2010/05/arp-packet.jpg)

*Imaged borrowed from OReilly's Understanding Linux Network Internals online via Google Images.


Note when PC1 sends the ARP request, the Ethernet (MAC) target address would be all zeroes because it is unknown. The reply sent by PC2 would replace the zeroes with PC1's Ethernet (MAC) address because it has learned from the ARP request packet.
