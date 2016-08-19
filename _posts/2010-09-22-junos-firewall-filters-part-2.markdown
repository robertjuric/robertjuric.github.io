---
author: robertjuric
comments: true
date: 2010-09-22 19:02:15+00:00
layout: post
slug: junos-firewall-filters-part-2
title: JUNOS Firewall Filters - Part 2
wordpress_id: 205
categories:
- Networking
tags:
- filter
- firewall
- Juniper
- JUNOS
---

As promised I'm following up with my [previous post](http://robertj.wordpress.com/2010/09/19/junos-firewall-filters/) regarding firewall filters and writing about implementing firewall filters. 

A brief recap: there are 3 different types of firewall filters available in JUNOS: port, vlan, and router (layer3). These are configured under the appropriate family (inet or ethernet-switching) in the FIREWALL hierarchy. The filters contain terms (up to 2048 of them) which are built like if/then statements which contain match conditions (the _if_ part) and actions (the _then_ part). Now let's get to it.

**Applying Port Filters:**

    
    interfaces {
       <interface name> {
          unit 0 {
             family ethernet-switching {
                filter input <filtername>
             }
          }
       }
    }


Notice that, as mentioned in my previous post, port filters are only able to be applied to ingress traffic on L2 interfaces.

**Applying Router (Layer 3) Filters:**

    
    interfaces {
       <interface name> {
          unit 0 {
             family inet {
                filter [input/ouput] <filtername>
             }
          }
       }
    }


Note here the option to filter on ingress or egress traffic of the L3 interface. It's also the same setup as a Port filter, but applied at the _inet_ family instead of _ethernet-switching_.

**Applying VLAN Filters:**

    
    vlans {
       <vlan name> {
          filter [input/ouput] <filtername>
       }
    }



A quick word on processing order, when processing Layer 2 packets(switched), the firewall filters are applied in this order:
1. Ingress Port
2. Ingress VLAN
3. Egress VLAN

When processing Layer 3 packets (routed), the firewall filters are applied in this order:
1. Ingress Port
2. Ingress VLAN
3. Ingress Router
4. Egress Router
5. Egress VLAN

I failed to mention this in my previous post, but if you're unhappy with how the terms are arranged in a filter, (since they are processed in order) you can simply re-arrange them instead of rewriting the entire filter:

    
    
    [edit firewall filter family inet filter dummyfilter]
    root@switch# insert term <term1> before term <term2>
    



Well that wraps it up for firewall filters. I sit for the JNCIA-EX exam next week, so you can expect to see a handful of basic switching posts coming soon.
