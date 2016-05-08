---
author: robertjuric
comments: true
date: 2011-03-26 02:20:08+00:00
layout: post
slug: junos-destination-nat-as-port-forwarding
title: JUNOS - Destination NAT as Port Forwarding
wordpress_id: 419
categories:
- Juniper
- Networking
tags:
- Destination NAT
- Juniper
- JUNOS
- NAT
- SRX
---

I recently moved into a new house and a new cable Internet provider. I decided I'd take the opportunity to upgrade my home network and implement a Juniper SRX100 (I <3 JUNOS) as my Internet router/firewall.


[![](http://robertj.files.wordpress.com/2011/03/img_0963.jpg?w=300)](http://robertj.files.wordpress.com/2011/03/img_0963.jpg)




The initial setup of the SRX100 was surprisingly pain-free. I had to reset it to factory defaults after I had been using it for some labbing, and the factory default config worked pretty much out of the box. I run a few private servers at my house and needed to setup some port forwarding as I only have a single public IP address. You can easily accomplish this with JUNOS Destination NAT, and I would like to share with you some interesting quirks I found in its implementation.




Destination NAT works by specifying NAT rules based upon a packet's destination address (and port). By configuring NAT rules based on the destination address of your public IP address you're able to to redirect a certain address or port to a private internal address of your choosing. In the case of the standard home user you would specify rules for different ports destined (incoming) to your single public IP address to be translated to multiple private addresses (one-to-many NAT). If you had multiple public IP addresses you could configure the different address to be redirected to multiple private addresses (many-to-many NAT).




Configuration of Destination NAT is also relatively simple; it only requires a destination pool for your internal server(s), NAT rules, and a security policy to allow traffic to flow between the zones.




I first configured a single pool for my internal server's IP address:




    
    set security nat destination pool <pool_name> address 192.168.1.5/32




Next I began to configure NAT rules for each port I wanted to forward. This is also where it got interesting. The syntax for creating NAT rules is:




    
    set security nat destination rule-set <rule_set_name> from zone untrust
    set security nat destination rule-set <rule_set_name> rule <rule_name> match destination-address 0.0.0.0/0
    set security nat destination rule-set <rule_set_name> rule <rule_name> match destination-port <#>
    set security nat destination rule-set <rule_set_name> rule <rule_name> then destination-nat pool <pool_name>
    




To keep things straightforward I named my rule-set "untrust-to-trust" and the rule names to match the port number (i.e. Port1024). Setting the destination-address to 0/0 is not in the documentation, but it will work if you rely on a dynamically assigned public IP address like I do. Another issue I ran into was a limitation on the number of rules allowed in a rule-set. Any JUNOS code version before 10.1R1 has a limitation of 8 rules per rule-set.  You can see how this could become a problem if you're attempting to forward a range of IP addresses. A code upgrade resolved this problem for me and extended the limitation to 512 rules per rule-set. Unfortunately there is no way to configure destination NAT for a range of ports, each rule is limited to a single port definition.




The final step requires configuring a security policy and address book entry to allow traffic to cross zones:




    
    set security zones security-zone trust address-book address <address_name> 192.168.1.5/32
    set security policies from-zone untrust to-zone trust policy <policy_name> match source-address any
    set security policies from-zone untrust to-zone trust policy <policy_name> match destination-address <address_name>
    set security policies from-zone untrust to-zone trust policy <policy_name> match application any
    set security policies from-zone untrust to-zone trust policy <policy_name> then permit
    




You can obviously keep your security tighter by specifying an application instead of using 'application any'.  The NAT is applied before the security policies are checked which is why the NAT rule's destination address is your public IP address and the security policy's destination address is the private internal IP address.




By using Destination NAT in JUNOS  you can effectively implement port forwarding for your home network. I'm satisfied with my SRX as a home router/firewall and enjoy the flexibility it brings.
