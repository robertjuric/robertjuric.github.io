---
author: admin
comments: true
date: 2013-12-21 12:37:04+00:00
layout: post
slug: redistribute-connected
title: Redistribute Connected
wordpress_id: 940
categories:
- Networking
tags:
- Cisco
- LSA
- OSPF
---

I've been building a lab scenario out for my next Pluralsight course and I discovered some interesting (or at least new to me) behavior with Cisco OSPF. I've already known about the different OSPF router types (ABR, ASBR), but it never really clicked for me until I saw it in real life.

The lab setup is very simple, a Juniper EX4200 connected to two Cisco 1721s running OSPF. Since I'm doing the labs on the Juniper EX4200, I was going to preconfigure the Cisco routers to already have OSPF configured. I configured the L3 interfaces and a network statement for the physical and the RID loopback. I also configured a loopback interface so I could generate some extra routes to the Juniper. I thought I would keep things simple so I just added the 'redistribute connected' command under the OSPF configuration without adding additional network statements. (This is me being lazy.)

When I logged into the EX4200 to verify the routes were working I noticed that one of the loopback routes I configured was showing up as an OSPF external route [OSPF/150]:
`192.168.10.1/32 *[OSPF/150] 00:00:17, metric 20, tag 0
> to 10.10.2.2 via ge-0/0/0.0
192.168.50.0/24 *[OSPF/150] 00:00:12, metric 20, tag 0
> to 10.10.3.2 via ge-0/0/23.0
`

So I started digging into the Cisco router to see why it was displaying the RID loopback, which had a network statement as an OSPF internal route, and the one being redistributed was showing up as external. A quick show ip ospf command and I had my lightbulb moment:
`Cisco-RTR#sh ip ospf 1
Routing Process "ospf 1" with ID 2.2.2.2
Supports only single TOS(TOS0) routes
Supports opaque LSA
Supports Link-local Signaling (LLS)
Supports area transit capability
** It is an autonomous system boundary router
Redistributing External Routes from,
connected**
Initial SPF schedule delay 5000 msecs`

That's when it hit me that the redistributed routes were automatically advertised as external. Which when I started thinking it made perfect sense. If we think about what the OSPF 'network' statement is actually doing; which is enabling OSPF and adding the interface(s) to the topology. Then it makes sense that my redistributed loopback interface wasn't actually part of the OSPF topology, it was just a network being advertised into the topology. I guess I was just thinking that since it was still a local interface on a router running OSPF that it wouldn't be considered 'external'. That's what I get for thinking. **The ASBR function is to redistribute routes from other protocols, even if that protocol is directly connected.**

When I was doing research into this behavior I found [this Cisco link](http://www.cisco.com/en/US/tech/tk365/technologies_tech_note09186a0080094707.shtml). So, I added a network statement to cover my additional loopback interface. This corrected the route preference on the Juniper EX4200, but I noticed that the Cisco router still considered itself an ASBR. So even if you have network statements covering all of the interfaces on the router, if 'redistribute connected' is still configured then the router will act as an ASBR.

This might be a 'duh' moment for most other engineers, and when I look back, it is. But it was a lightbulb moment for me to really understand the Type 5 LSA and the function of the ASBR in OSPF.
