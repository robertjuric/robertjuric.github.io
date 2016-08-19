---
author: robertjuric
comments: true
date: 2010-08-10 01:35:48+00:00
layout: post
slug: eigrp-neighbor-notes
title: EIGRP Neighbor Notes
wordpress_id: 133
categories:
- Networking
---

## Process Overview:





	
  * **Neighbor Discovery** - Hellos sent to multicast address 224.0.0.10 based on Hello Timer Interval. If requirements are met routers become neighbors.



	
  * **Initial Topology Exchange** - Once routers become neighbors they will complete a full topology exchange. Topology information is sent via Update messages sent to multicast address 224.0.0.10 via RTP (Reliable Transport Protocol).



	
  * **Route Selection** - Router calculates the metric for each route and populates the routing table with the best routes. If multiple equal cost routes exist EIGRP will put both routes in the routing table and perform load-balancing. If multiple unequal routes exist, higher cost routes will be kept in the topology table as Feasible Successor routes for faster convergence if a topology change occurs.



	
  * **Topology Change Updates** - After the initial exchange, topology updates are only sent when a change occurs to the topology.




## Requirements to become neighbors:





	
  * Same Subnet

	
  * Hello or ACK received

	
  * AS Numbers Match

	
  * Metrics (K Values) Match

	
  * For Topology Updates: Authentication Matches (If Applicable)




## Verification Commands:


`show ip eigrp interfaces
`show ip protocols
`show ip eigrp neighbors
`show ip eigrp topology
`show ip route`````
