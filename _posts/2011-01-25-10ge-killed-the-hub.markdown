---
author: robertjuric
comments: true
date: 2011-01-25 17:49:00+00:00
layout: post
slug: 10ge-killed-the-hub
title: 10GE Killed the Hub
wordpress_id: 406
categories:
- Insights
- Networking
tags:
- 10GE
- '802.3'
- CSMA/CD
- Ethernet
---

I was talking to a fellow network engineer the other day showing off our new 10GE equipment and he made a comment how 10GE was a totally different technology than standard Ethernet because it did not use CSMA/CD. I acknowledged I didn't know what he was talking about and ran off to do some research, at which point I feel I fell into the Ethernet rabbit hole. I thought I would share my finding with you.

Let's get down and dirty with a quick overview of the base layers of the OSI model:

Ethernet (IEEE 802.3) is compromised of both physical and data-link layer protocols that define the technology we network engineers all know and love. 

The data link layer consists of two sub-layers. The Medium Access Control (MAC) sub-layer and the Logical Link Control (LLC) sub-layer. The MAC is a communication protocol sub-layer that provides addressing and channel access control mechanisms. In other words the MAC layer is pretty much our beloved 48-bit MAC addresses and CSMA/CD. MAC operates between the LLC sublayer and the physical layer to emulate full duplex communication. The LLC sublayer provides multiplexing so that muliple network protocols can coexist on the same media (such as IP & IPX), and also provides flow control mechanisms. The LLC sub-layer acts as an interface between the MAC sublayer and the network layer.

Duplex is a method that two units communicate with each other in both directions. Half-duplex provides communication in both directions but only one direction at a time. Full duplex allows bi-directional communication simultaneously. Half-duplex can lead to collisions and re-transmissions (hence the need for CSMA/CD.) Full duplex allows communication simultaneously in both directions so there are no collisions. In full-duplex communication, a MAC layer is not necessary though it is still implemented for hardware addressing purposes. A hub is a good example of half-duplex communication.

Carrier Sense Multiple Acces (CSMA) is a MAC protocol in which a node verifies the absense of traffic before transmitting on a shared medium. CSMA with Collision Detection (CSMA/CD) is a modification of CSMA that improves performance by terminating transmission as soon as a collision is detected. 

Flow Control is implemented in Ethernet with 802.1x when one station is sending traffic faster than the other 
station can receive it. When this happens a station sends a PAUSE command to a bridge group address and all data is stopped. On a side note: This messes with Classes of Service (802.1p) because all priorities are stopped. One of the new Data Center Bridging protocols, Priority Flow Control (802.1Qbb), attempts to address this issue by indicating pause time for each priority separately.

Now back to the original comment:
1000BASE-T supports autonegotiation of half-duplex communication when necessary. 10GE defines only full duplex links, so half-duplex and therefore CSMA/CD do not exist. The statement by my fellow engineer is correct, 10GE is missing CSMA/CD. However since Gigabit Ethernet, when operating in full duplex, doesn't utilize CSMA/CD either, I now consider it a bold statement that 10GE is totally different from standard Gig Ethernet. It could be said however that 10GE killed the hub!
