---
author: robertjuric
comments: true
date: 2011-09-29 18:59:16+00:00
layout: post
slug: security-policy-and-address-groups
title: Security Policy and Address Groups
wordpress_id: 510
categories:
- Networking
tags:
- Juniper
- JUNOS
- policy
- ScreenOS
- security
---

I recently sat through a Juniper Security (JSEC) training course and learned something new about security policy regarding the objects that are able to be referenced in policy. When I got back to work I had a few rules I needed to add and some that needed to be removed so I thought it would be a good point to make the switch. I am talking about the use of address groups in security policy. Before we can reference a prefix in security policy it must be created as an address object. These address objects are stored in an address-book _per_ security zone.  We can also create address-sets (Junos) and address groups (ScreenOS) which are basically groups consisting of individual address members. When we create a security policy, instead of creating a policy for each address entry, we can reference the address-set/address group to apply the policy to like entries. This allows us to change individual entries for a policy without directly affecting the policy. For example, I have a group of 5 servers that need web access to download updates, but I don't want the entire zone to have Internet access. Instead of creating 5 individual policies I can create a single policy and refence an address-set/address group which consists of the 5 server address objects. In the future if I ever decide to give a new server Internet access or I need to remove Internet access for a server, I don't have to worry about cluttered policies, I can just add or remove an address to the address-set/address group.


## Junos


We first need to create our address objects:

    
    set security zones security-zone *ZoneName* address-book *AddressName* x.x.x.x/y


Then we need to create our address-set:

    
    set security zones security-zone *ZoneName* address-book address-set *SetName* address *AddressName*


A few caveats, an address-set can only exist when there multiple addresses in the address-book. An address-set can only contain addresses from the same security zone, and also the address-set name cannot be the same as an existing address name.


## ScreenOS


First create the address objects:

    
    set address *ZoneName* *AddressName* x.x.x.x/y *Comment*


Then create the address group:

    
    set group address *ZoneName* *GroupName* *Comment*
    set group address *ZoneName* *GroupName* add *AddressName*


One thing I noticed in the Junos TechPubs was that when you reference an address-set in policy Junos creates an internal rule for each member, as well as each service. So if you create a policy which references an address-set for both source and destination and also a service group, Junos actually creates multiple internal rules. If you don't pay attention to the size of your groups you could exceed policy resources even though it appears you have a relatively small number of configured policies.

The main point I want to convey isn't necessarily how to configure address sets/groups, but to motivate you to use them. It's standard practice in the Windows AD realm where you rarely want to give an individual user rights to a server, it's much cleaner to give a security group rights to the server and then manage the membership of that group. This is particularly helpful if the same group will be referenced on multiple servers. The same concept applies to our firewall security policies.

So go forth and simplify your firewall rules...
