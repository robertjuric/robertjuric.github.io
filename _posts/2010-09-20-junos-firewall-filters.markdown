---
author: robertjuric
comments: true
date: 2010-09-20 02:41:23+00:00
layout: post
slug: junos-firewall-filters
title: JUNOS Firewall Filters
wordpress_id: 184
categories:
- Juniper
- Networking
tags:
- CLI
- filter
- firewall
- Juniper
- JUNOS
---

  
They're called ACLs or Access Control Lists in IOS, but in JUNOS they're called firewall filters. While they appear more complex than a simple ACL line, they do offer much more control and granularity over the commands. JUNOS has 3 different type of firewall filters: port (layer 2) filters, vlan filters, and router (layer 3) filters. Port firewall filters are applied to layer 2 switch ports, only in the ingress direction. VLAN firewall filters can be applied to both ingress and egress directions on a VLAN. Router firewall filters can be applied to the ingress or egress directions on layer 3 interfaces and RVIs (routed vlan interfaces), they can also be applied to the ingress direction on a loopback interface. 

Like IOS, you must configure the firewall filter and then apply it at the correct level. You are only able to apply one firewall filter per port/vlan/router interface, per direction. However the filter is able to support up to 2,048 terms. The firewall filter syntax is as follows:

    
    <code>
    firewall {
       family [inet/ethernet-switching] {
          filter <filtername> {
             term <termname> {
                from {
                   <match conditions>
                }
                then {
                   <action>
                }
             }
          }
       }
    }</code>


  
  

You can check out [this link from Juniper](http://jnpr.net/techpubs/en_US/junos/topics/reference/requirements/firewall-filter-ex-series-match-conditions.html) for a nice list of match conditions and actions. As you can see in the link, there are terminating actions and modifying actions.

To match multiple conditions within a term you can use logical AND/OR statements. If the term contains common match statements (such as multiple protocols), it is treated as a logical OR statement. Uncommon match statements are as treated logical AND statements. For example:

    
    <code>
    from {
       protocol [tcp udp];
       source-port 123;
    }</code>


In this example, the protocol match statement is treated like a logical OR statement, while the combination protocol/source-port  is treated as a logical AND statement.

As a packet is processed through the filter, it is processed by each term in order. If there is a match in a term, and a terminating action is specified, no other terms are processed. If there is an action modifier, but no terminating action, it will default to the action of _accept_. If there is no match on the filter, the packet is discarded (implicit discard). 

That should be a quick overview of firewall filters in JUNOS. I will pick up next time with applying firewall filters.
