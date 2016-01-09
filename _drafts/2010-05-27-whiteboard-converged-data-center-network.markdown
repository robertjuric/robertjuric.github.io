---
author: robertjuric
comments: true
date: 2010-05-27 21:24:03+00:00
layout: post
slug: whiteboard-converged-data-center-network
title: Whiteboard - Converged Data Center Network
wordpress_id: 109
categories:
- Insights
- Networking
tags:
- CEE
- DCB
- DCE
- FCoE
---

![](http://robertj.files.wordpress.com/2010/05/100_06421.jpg?w=1024)

Let's look at this from bottom to the top:

Layer 1 of the OSI Model gives us 10GE or 10 Gigabit Ethernet. 10GE is standardized as IEEE 802.3-2008 which covers the various physical layer flavors such as 10GBASE-SR.

Layer 2 defines our Data Link protocols such as FiberChannel and Ethernet; we'll focus on the Ethernet protocol. Ethernet has been stretched out to cover many various enhancements under IEEE 802.1. In the converged data center network there is a demand for improvements over our standard "lossy" Ethernet. This is where DCB, or Data Center Bridging, comes into play. DCB is a collection of enhancements to the 802.1 Ethernet standard that can transform Ethernet to "lossless". These enhancements include 802.1Qau, 802.1Qaz, and 802.1Qbb. Fore more information on these enhancements check out the[ IEEE DCB Task Group](http://www.ieee802.org/1/pages/dcbridges.html). DCB is the term for these enhancements used by the IEEE. DCE, or Data Center Ethernet, is Cisco's trademarked term based on these enhancements. CEE, or Converged Enhanced Ethernet, is trademarked by IBM and used by companies such as Juniper. I think it is important to note that while all of these terms mean the same thing, none have been ratified as a standard.

Layer 3 is where FCoE, or Fiber Channel over Ethernet, calls home. FCoE is the encapsulation of Fiber Channel frames into Ethernet frames. Because Fiber Channel is a loss-less protocol it requires the enhancements to Ethernet as found in DCB. One important item to note is that because FCoE lives at Layer 3 it is not able to route across IP networks.

I threw iSCSI in there where it lives in Layer 5 so that you can see how iSCSI traffic is able to route across our IP networks. Hopefully seeing this drawn out based on OSI layers can give you a better grasp of the technologies and their buzzwords.

*Disclaimer: I am by no means any time of expert in these matters. These postings are mainly for my own benefit so that I can better understand what I'm working with.
