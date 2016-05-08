---
author: robertjuric
comments: true
date: 2010-09-27 01:51:17+00:00
layout: post
slug: junos-vlans-and-trunking
title: JUNOS VLANs and Trunking
wordpress_id: 215
categories:
- Juniper
- Networking
tags:
- Juniper JUNOS CLI VLAN trunk
---

Today, I'm going to give a quick introduction to VLANs and trunking in JUNOS. VLANs in JUNOS work almost opposite like they do in IOS. You first create a VLAN with a vlan-name and then you assign a vlan-id (dot1q tag). By default all switch-ports are assigned to the 'default' vlan which is untagged (no vlan-id is assigned). 

To create a VLAN:

    
    <code>set vlans <vlan-name> vlan-id <#></code>



You then assign interfaces to a VLAN individually:

    
    <code>set interfaces <interface-name> unit 0 family ethernet-switching 
    vlan members <vlan-name></code>



Fairly straight-forward right? Now on to trunking, which differs a good bit from what we are used to with IOS. By default, JUNOS does not specify a native-vlan. Trunks, by default, do not support any (allow) any VLANs. Trunks also do no auto-negotiate by default. 

To set an interface as a trunk:

    
    <code>set interfaces <interface-name> unit 0 family ethernet-switching 
    port-mode trunk</code>


You can then either explicitly allow individual VLANs, or you can allow all VLANs on the trunk:

    
    <code>set interfaces <interface-name> unit 0 family ethernet-switching
    vlan members all</code>



If you would like to trunk to a Cisco device, you must first create a "native" vlan (it would help to name it 'native') with a vlan-id of 1 (unless you've changed the native-vlan on the Cisco device). You can then specify the native-vlan on the trunk interface:

    
    <code>set interfaces <interface-name> unit 0 family ethernet-switching
    native-vlan-id <#></code>



I know that this does not appear to be a very easy way of assigning interfaces to VLANs, but at this point in my study it's all that I've got. I am going to do some more research into this, as I'm sure there is a more efficient way of doing things. 
