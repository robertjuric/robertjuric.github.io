---
author: robertjuric
comments: true
date: 2010-10-05 20:51:37+00:00
layout: post
slug: junos-is-out-of-this-world
title: JUNOS is Out of This World!!
wordpress_id: 241
categories:
- Juniper
- Networking
tags:
- Juniper
- JUNOS
- Martian
---

<code>show route martians</code>


Say what? Martians? Aliens? Ok ok, I know it's corny but I learned something new today and it made me grin.

So it turns out Martian addresses are not how you get a letter to Mars, instead they are network addresses for which JUNOS decides to ignore all routing information. These addresses are never added to the routing table, and usually are only present by misconfiguration/mistake.

The IPv4 addresses which are by default considered Martian addresses are:



	
  * 0.0.0.0/8

	
  * 127.0.0.0/8

	
  * 128.0.0.0/16

	
  * 191.255.0.0/16

	
  * 192.0.0.0/24

	
  * 223.255.255.0/24

	
  * 240.0.0.0/4


In IPv6:

	
  * Loopback address (::1)

	
  * Reserved and unassigned prefixes from [RFC 2373](http://www.ietf.org/rfc/rfc2373.txt) (which apparently has been obsoleted by [RFC 4291](http://www.ietf.org/rfc/rfc4291.txt))

	
  * Link-local unicast prefix (fe80::/100)


You can also add additional prefixes to the list of Martian addresses by setting prefix lists in the `[edit routing-options martians]` hierarchy. You are able to specify the prefix, bit mask, and a match type. The match types you can choose from are:



	
  * exact

	
  * longer

	
  * orlonger

	
  * prefix-length-range

	
  * through

	
  * upto


You can also specify the `allow` command to remove a prefix from the Martian address list.

Some examples:

    
    <code>
    [edit routing-options]
    root@Router# set martians 31/8 orlonger
    root@Router# set martians 240/4 orlonger allow
    </code>



    
    <code>
    root@Router> show route martians
    root@Router> show route martians table inet.0
    </code>



You learn something new every day.
