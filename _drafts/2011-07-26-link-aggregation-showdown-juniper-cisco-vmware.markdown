---
author: robertjuric
comments: true
date: 2011-07-26 00:54:05+00:00
layout: post
slug: link-aggregation-showdown-juniper-cisco-vmware
title: Link Aggregation Showdown - Juniper, Cisco, VMware
wordpress_id: 490
categories:
- Cisco
- Juniper
- Networking
tags:
- Cisco
- Etherchannel
- Juniper
- LACP
- LAG
- NIC Teaming
- PAgP
- VMware
---

A friend and I got into a discussion regarding supported link aggregation methods between VMware and various networking vendors. As I was trying to inform him of the differences, I was sure to do my research and I thought I would share.

He was using Cisco switches and wanted to run LACP because he thought that Cisco's Etherchannel did not have any negotiation protocol unless you used LACP. He then thought that VMware would support LACP, since it was an open standard after-all. We have both been bitten by misconfigured link aggregations when connecting to VMware. It turns out he was wrong here on both fronts. First of all, VMware only supports static configuration for link aggregation (NIC teaming as they call it.) Secondly, Cisco's Etherchannel does support negotiation using PAgP(Cisco Proprietary), if configured to do so. I just wanted to point this out for future reference. So I put together a quick cheat sheet for him to use building link aggregation bundles across vendors.

Static Link Aggregation:
VMware - Configure the port group NIC teaming to "Route Based on IP Hash"
Cisco - 
    
    port-channel # mode on


Juniper - 
    
    set interface <name> ether-options 802.3ad ae#



Link Aggregation Negotiation (LACP):
VMware - Not Supported
Cisco - 
    
    port-channel # mode active


Juniper - The Static Configuration + 
    
    set interface ae# aggregated-ether-options lacp active



Link Aggregation Negotiation (PAgP):
VMware - Not Supported
Cisco - 
    
    port-channel # mode desirable


Juniper - Not Supported

There are also passive configuration options for both LACP  and PAgP, however I prefer to always statically define my port roles. These are also not complete configs for either Cisco or Juniper, I merely wanted to point out the differences in the commands between the different vendors.
