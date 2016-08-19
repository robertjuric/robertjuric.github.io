---
author: robertjuric
comments: true
date: 2010-11-09 19:58:19+00:00
layout: post
slug: vlan-access-lists
title: IOS - VLAN Access Lists
wordpress_id: 301
categories:
- Networking
tags:
- ccnp
- Cisco
- ios
- VACL
---

So we all know how to filter traffic as it passes between VLANs or between L3 interfaces using standard ACLs. But what do we do about filtering traffic withing a VLAN? Say we want to isolate a single host by disallowing it to communicate with other hosts in the same subnet? Step in the VACL. The switch TCAM performs the VACL matching and action as packets are switched within the VLAN or routed to a different VLAN. There is no need to specify inbound or outbound direction. VACLs are configured as VLAN access maps much like a route map. 

You first have to configure the VACL:

    
    Switch(config)# vlan access-map <map-name> [sequence-number]


The access-maps are processed in sequence-number order. You are able to have multiple matching conditions followed by an action.

    
    Switch(config-access-map)# match ip address {acl-number | acl-name}
    Switch(config-access-map)# match mac address <acl-name>
    Switch(config-access-map)# action {drop | forward | capture} | redirect <port>}



The next step is to apply the VACL to the VLAN:

    
    Switch(config)# vlan filter <map-name> vlan-list <vlan-list>


You are able to apply the VACL to multiple VLANs using the vlan-list.

This has been a quick and dirty look at VACLs, another tool for your toolbox.
