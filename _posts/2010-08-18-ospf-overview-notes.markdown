---
author: robertjuric
comments: true
date: 2010-08-18 03:37:25+00:00
layout: post
slug: ospf-overview-notes
title: OSPF Overview Notes
wordpress_id: 155
categories:
- Networking
---

Summary:
Routers form neighbors and send LSAs (link state announcements). As a router receives LSAs, it uses them to build the LSDB (link state database), applies SPF algorithm to LSDB to select the best routes to populate the routing table.

1. Neighbor discovery
Multicast hellos sent to 224.0.0.5 for neighbor discovery
Must meet requirements:
Same subnet
Hello/dead intervals match
Ip mtu match
Pass authentication
Unique router ids
If routers meet requirements they move to the 2-way state and are then able to move forward with topology exchange. Hellos continue to be sent to monitor responsiveness.

2. Topology exchange
After neighbors form, they exchange LSA headers via DD (database description) messages. The DD messages allow the router to learn what it does not know so that it can request only the complete LSAs it needs. For missing LSAs the router sends a LSR (link state request) which is replied with by a LSU (link state update) and then acknowledged by a LSAck (link state acknowledgement). When all LSAs have been exchanged and both routers have identical LSDBs, they move to the fully adjacent state.

When a DR (designated router) exists, all routers exchange databases only with the DR via a Type 2 LSA psuedo node. DROther (non-dr) nodes send updates to multicast 224.0.0.6 which are processed only by the DR and BDR. Essentially all DROther routers exchange databases only with the DR which then relays back to the rest of the DROthers. This is why only neighborships with the DR will be FULL while neighborships between DROthers remain in a 2-way state.

3. Route selection
Router searches LSDB to find all possible routes to a destination. For each route, the router adds up OSPF interface costs for all outgoing interfaces. The router will then pick the route with the lowest total cost and add the route to the routing table.
