---
author: admin
comments: true
date: 2012-08-14 13:24:36+00:00
layout: post
slug: cisco-vacl-for-ids-monitoring
title: Cisco VACL for IDS Monitoring
wordpress_id: 691
categories:
- Networking
tags:
- Cisco
- ios
- VACL
---

Instead of using a SPAN or VSPAN port to capture traffic for an IDS it is more efficient to target specific traffic for monitoring. This can be completed at a much more granular level by using a VACL. VACLs are more efficient because they only target specific traffic and they are also enforced in hardware, so the performance impact is lower.  To setup a VACL for IDS monitoring you need 4 main components: an access-list identifying interesting traffic, a VLAN access-map to capture the traffic, a VLAN filter to apply the access-map, and a capture port to send the traffic to.

You actually need 2 ACLs, one to identify interesting traffic and another to permit all traffic, the latter will be crucial in building the access-map.

    
    ip access-list extended MONITOR
      permit ip 192.0.2.0 0.0.0.255 any
      permit ip host 198.51.100.23 any
      permit ip host 198.51.100.24 any
    exit
    
    ip access-list extended ALL_TRAFFIC
      permit ip any any
    exit



We then use these ACLs to build our VLAN access-map.

    
    vlan access-map MONITOR 10
      match ip address MONITOR
      action forward capture
    vlan access-map MONITOR 20
      match ip address ALL_TRAFFIC
      action forward
    exit


The key here is the "action forward capture", this will continue to forward the packet and also send it to our capture port. We also must add another statement so that all other legitimate traffic will continue to be forwarded. If we did not, traffic that wasn't matched as interesting by the first ACL would just get dropped by the access-map.

Next we need to apply our access-map to the VLANs we wish to monitor.

    
    vlan filter MONITOR vlan-list 6,28,30,58



And finally configure our capture port which will have the IDS tap connected to it.

    
    interface Gi1/20
      description IDS Port
      switchport capture allowed vlan 6,28,30,58
      switchport capture



"Switchport capture" has some interesting effects on the interface. Unless you want to apply dot1q tags no further interface config is necessary. If you want to apply the dot1q tags, configure the port as a trunk without negotiation. The capture port only supports egress traffic; if you you do a show interface you will see (along with only packets output): 

    
    6509#  sh int gi1/20
    GigabitEthernet1/20 is up, line protocol is down (monitoring)
      ...



Finally, we can verify our config by checking the access-map and the vlan filter:

    
    6509#sh vlan access-map MONITOR
    Vlan access-map "MONITOR"  10
            match: ip address MONITOR
            action: forward capture
    Vlan access-map "MONITOR"  20
            match: ip address ALL_TRAFFIC
            action: forward
    6509#sh vlan filter
    VLAN Map MONITOR:
            Configured on VLANs:  6,28,30,58
                Active on VLANs:  6,28,30,58



More posts to follow, I'm planning on kick-starting this blog again.

PS, did you notice my use of [RFC 5737 address space](http://tools.ietf.org/html/rfc5737)?
