---
author: admin
comments: true
date: 2013-06-21 18:23:13+00:00
layout: post
slug: bits-on-a-wire
title: Bits on a Wire
wordpress_id: 896
categories:
- Insights
- Networking
tags:
- MSS
- MTU
- Network
---

Bits, Frames, Packets, Streams, Datagrams; we have many terms for what basically involves getting information sent across a transmission medium. However, I like details, and I think that the terms should be used correctly. As somebody once told me, "Words have meanings!" So let's get a few things straight:

**Bits** - The last logical unit of data before the information is turned into an electrical or optical signal.

**Frames** - Layer 2 data units used to transfer data between adjacent network nodes. Provides frame synchronization (framing) and can provide error detection.

**Packets** - Used to establish the path which data travels from host-to-host. In the world of networking and the OSI model, this strictly refers to Layer 3 (IPv4 or IPv6).

Let's pause right here to make a special note between frames and packets and their uses. Frames are used at L2 to transfer data between adjacent nodes, while packets are used at L3 to transfer data between end-to-end hosts. What this means is that L2 frame headers(src/dst addresses) are rewritten at each node while L3 packet headers (src/dst addresses) do not change.

**Segments/Streams/Datagrams** - Used by Layer 4 protocols to provide end to end communication for applications. Datagram is a term used when delivery, time, or order are not guaranteed by the protocol, such as with UDP. Streams/segments are used by protocols such as TCP to guarantee end to end error free communication. A TCP stream is considered a data stream of bytes, not individual messages. If this single flow of bytes is too large to flow across the network in a single message, it can be fragmented into smaller pieces. A TCP segment would be an individual piece of the larger TCP stream. TCP streams offer many "services" for applications such as ordered delivery, guaranteed delivery, flow control, congestion avoidance, and even multiplexing.

As network engineers it's our job to ensure that end to end communication is available for the higher layers of our protocol stacks. Now that we know the correct terms for how data is encapsulated so that it can be transported across the network we should start to look at how this data fits across our network. How big are these TCP segments, datagrams, packets and frames flowing across our network? At first it may look like this doesn't matter but when a higher level protocol is trying to cram too much information into the pipes this can cause problems. Just picture connecting a garden hose to a fire hydrant. This is where MTU and MSS come into play.

MTU, Maximum Transmission Unit, is the maximum size of data that a network layer can forward. This means that MTU is/can be different for each layer (Ethernet MTU, IPv4 MTU). TCP uses MSS, Maximum Segment Size, to define the maximum size of data that it will include in an individual TCP segment which would make up a TCP stream. We will talk about MSS in a moment, for now let's focus on MTU.

Differentiating between the physical interface MTU (PHY MTU) and IP MTU can get a little confusing. The PHY MTU is the maximum allowed frame size (not counting the header) or in other words, the payload size. IP MTU is the maximum packet size allowed to transmit before fragmentation occurs. Increasing PHY MTU will also increase IP MTU. However decreasing IP MTU will not affect the interface PHY MTU. Why does this matter? In scenarios where we are adding additional headers such as GRE or MPLS. To accommodate the extra protocol overheard of something like GRE, you could lower the IP MTU by 24 bytes to leave room for the GRE overhead without exceeding the PHY MTU.

TCP MSS is the maximum size of data that TCP is willing to **RECEIVE** in a segment. When a TCP connection is established between two hosts they announce their TCP MSS to each other. It's interesting to note that TCP MSS isn't negotiated and doesn't necessarily have to match.

I'm going to leave you with this little graphic I've borrowed and want to give full credit to @PacketLife 's [blog post on MTU Manipulation](http://packetlife.net/blog/2008/nov/5/mtu-manipulation/):

[![MTUs](http://robertjuric.com/wp-content/uploads/2013/06/MTUs.png)](http://robertjuric.com/wp-content/uploads/2013/06/MTUs.png)

I may write some follow-on posts to talk about PMTUD or TCP Window Scaling, but for now I just wanted to get some definitions out of the way.
