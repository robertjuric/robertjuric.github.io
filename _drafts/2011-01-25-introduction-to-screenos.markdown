---
author: robertjuric
comments: true
date: 2011-01-25 02:22:33+00:00
layout: post
slug: introduction-to-screenos
title: Introduction to ScreenOS
wordpress_id: 397
categories:
- Juniper
- Networking
---

I feel like I haven't written in a while but I've been going through a job transition. I finally feel like I'm starting to settle in and I thought I would share some of my new experiences learning about the Juniper Netscreen firewalls.

Unlike a majority of Juniper firewall users, I've had no experience with Netscreen firewalls. I was a little a little off-balance trying to pick up the SCREENOS commands, but once I got going on it I found they made logical sense and were similar to the JUNOS commands (sometimes).

My new job involves standing up a new development "enclave" network. When I arrived, a few others had turns unsuccessfully trying to make the network go, so my first task was to get the firewall passing traffic. I spent a few hours that night researching the basics of SCREENOS and the next day I was able to get the packets flowing.

SCREENOS CLI is very similar to to JUNOS in that it uses 'set' commands to make configuration changes. SCREENOS also differs greatly in that it uses 'get' commands to view config snippets.


    
    get config
    get interface
    get policy


A few examples of the 'get' commands

I learned to imagine SCREENOS in layers. The foundation layer is the virtual-router, which closely resembles routing-instances in JUNOS. There are two system configured VRs: trust-vr and untrust-vr. The virtual routers are essentially routing table instances. The next layer is compromised of the security zones, which are by default assigned to the trust-vr virtual router. From here we need to step back and see how the Netscreen operates. It can operate in one of three modes, NAT mode, route mode, and transparent mode. I like to think of the 3rd layer of the Netscreen as the interfaces. When an interface is assigned to the Trust security zone it is by default configured in NAT mode, if you wish to configure straight routing you must explicitly configure it. Also, before you can assign an IP address to an interface, the interface must first be assigned to a security zone.  


    
    set int eth2/1 zone Trust
    set int eth2/1 ip 10.10.1.1/30
    set int eth2/1 route
    set int eth1/1 zone Untrust
    set int eth1/1 ip 10.10.2.1/30
    



Now that we have our virtual router, security zones, and interfaces configured, the next layer consists of the routing and security policies which will allow the traffic to flow. The first thing we can do is configure a static default route:


    
    set route 0.0.0.0/0 interface eth1/1 gateway 10.10.2.2



The next thing we need to do is configure a security policy to allow some traffic to flow through our security zones:


    
    set policy from Trust to Untrust any any any permit


This policy consists of a source zone, destination zone, source object, destination object, and control action. More simply it reads: From zone Trust to zone Untrust and from any address to any address using any protocol then allow. Like JUNOS, SCREENOS uses address objects any time it references a specific IP or range of IPs in a security policy. For instance:

    
    set address DMZ 192.168.75.1/32 "Exchange"
    set address Untrust 1.1.1.16/28 "SmartHost"
    set policy from Untrust to DMZ SmartHost Exchange smtp permit



That's pretty much the basics of getting the firewall to pass traffic. A quick note with the policies. If you do a 'get policy' it will list a policy id number next to each entry. You can then do 'unset policy id #' to remove policies. I just wanted to take a second to share with you my introduction to SCREENOS. I plan to write some more about it and the Netscreen firewalls as I spend more time working with them.
