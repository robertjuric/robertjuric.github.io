---
author: robertjuric
comments: true
date: 2011-02-24 11:42:48+00:00
layout: post
slug: what-is-qfabric
title: What is QFabric?
wordpress_id: 412
categories:
- Cisco
- Insights
- Juniper
- Networking
tags:
- Cisco
- FabricPath
- Juniper
- QFabric
- Stratus
---

Yesterday Juniper released another piece to the Project Stratus puzzle. Project Stratus is Juniper's attempt at a 3-2-1 process of flattening the data center hierarchy model. They introduced the new [QFX3500 switch](http://www.juniper.net/us/en/products-services/switching/qfx-series/qfx3500/), and the concept of [QFabric](http://www.juniper.net/us/en/local/pdf/whitepapers/2000384-en.pdf). As always, Twitter went crazy. I did enjoy the back and forth, it was educational listening to different perspectives from people much smarter than myself. A small debate broke out whether or not Juniper's QFabric was different from Cisco's FabricPath. I'm writing this article to hopefully entice some other opinions or blog replies. I'm sure not an expert, but I'm going to do my best to explain what I think the differences are. I'm not going to say one is better than the other, but to myself they do appear different.

**Hardware**

The QFX3500 is a 1U 48port 10GE switch, fully L2/L3 capable. This box seems similar to the Cisco Nexus 5548 switch. Not much to get excited about here, it was about time Juniper released something comparable.

Is there a major difference? I'm thinking no.

**The Design, Stratus, QFabric**

Project Stratus is supposed to consist of the QFX3500s linked back to a large chassis switch, which has yet to be released, using Juniper's new QSFP+ ports. This is supposed to yield an extension of the control plane(packets are processed once at ingress to the "fabric"), and a single point of management. This concept sounds to me like how the Nexus 7000 can now support Nexus 2000s hanging off of it. I see two differences here: <del>1. Cisco does it with standard SFP+ ports</del>(I made the mistake of not researching the standard QSFP+ ports Juniper uses), and 2. The Nexus 2000 is little more than a dumb line-card and does not do any packet processing. I'm not sure if the method how the Stratus/QFX3500 or the Nexus 7k/2k uplink is much different(i.e. fex-fabric ports vs. Juniper's yet to be released config info), I plan on doing some more research into that.

Is there a major difference? I think a few small details, but the design is quite similar.

**FabricPath = QFabric?**

According to Cisco, FabricPath "brings the stability and scalability of routing to Layer 2." FabricPath is more of a L2 domain control innovation, or a stop-gap to TRILL/SPB, than a control plane extension. However, if you look at the FEX Fabric that links Nexus 5k>2k or 7k>2k, now that is a more accurate comparison to QFabric. With FEX Fabric and QFabric, the packet is processed once, and forwarded directly to it's destination. FabricPath still relies on multiple switch hops to operate. I think Juniper looks at from the view that if the network operates like one giant switch fabric, then there are no multiple hops and therefore no need for TRILL/SPB.

Is FabricPath = QFabric? I don't think so. But it does sound similar to Cisco's FEX Fabric.

**Vaporware?**

Yes. Juniper has only released the QFX3500 switch, which today can be used as a standard 10GE ToR switch. For Project Stratus to come together they still have to release their director chassis switch which will then use QFabric to interconnect the QFX3500s.

**Final Thoughts**

First I'm a little disappointed in Juniper for being so vague. If I hadn't already talked to our Juniper reps about this release, I think I would be even more confused. Is QFabric similar to FabricPath? I still don't think so, but I'm open to being educated. Is QFabric revolutionary? At first I thought it was, but after seeing the Twitter-discussion, I'm not so sure now. Please feel free to reply/inform/complain, I'm on Twitter [@robertjuric](http://twitter.com/#!/robertjuric).
