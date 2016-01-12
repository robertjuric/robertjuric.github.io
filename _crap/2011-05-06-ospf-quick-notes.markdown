---
author: robertjuric
comments: true
date: 2011-05-06 13:10:40+00:00
layout: post
slug: ospf-quick-notes
title: OSPF Quick Notes
wordpress_id: 438
categories:
- Cisco
- Juniper
- Networking
---

OSPFv2 - [RFC2328
](http://www.ietf.org/rfc/rfc2328.txt)OSPFv3 - [RFC5340](http://tools.ietf.org/rfc/rfc5340.txt)
Opaque LSA Option - [RFC2370](http://tools.ietf.org/rfc/rfc2370.txt)

All OSPF Routers Multicast Address - 224.0.0.5
All OSPF DRs Multicast Address - 224.0.0.6

OSPF Packet Types



	
  * Type 1 - Hello - Used to discover & maintain neighbors

	
  * Type 2 - DBD - (Data Base Descriptor) Used in adjacency formation process to exchange LSAs

	
  * Type 3 - LSR - (Link State Request) Used when LSDB becomes stale or is missing an LSA

	
  * Type 4 - LSU - (Link State Update) Contains LSAs

	
  * Type 5 - LSAck - (Link State Acknowledgement) Acknowledges LSAs
*An individual link-state acknowledgment packet can contain an acknowledgment for a single link-state update packet or for multiple link-state update packets. - JNCIS-ENT Routing Study Guide 
OSPF Adjacency States

	
  * Down - Initial state, waiting for start event

	
  * Init - Hello sent but bidirectional communication not established

	
  * 2Way - Hello received, bidirectional communication established

	
  * ExStart - Routers negotiate who in in charge of LSDB sync

	
  * Exchange - Exchange LSA headers, if missing data send LSR

	
  * Full - LSDBs fully sync'd


OSPF Router Types

	
  * DR - Designated Router - Represent multiple routers on a broadcast link

	
  * DR forms Full adjancies with all DROthers and the BDR

	
  * BDR - Backup Designated Router

	
  * BDR forms Full adjancies with the DR and all DROthers, does not advertise learned link state info

	
  * DROther - Other routers on a broadcast link

	
  * DROthers form 2Way adjacencies with each other

	
  * ABR - Area Border Router - Router with links to at least 2 different areas

	
  * ASBR - Autonomous System Boundary Router - Injects information from outside the OSPF AS

	
  * Backbone Router - Any router with a link to the Backbone Area

	
  * Internal Router - Any router with all links in the same area


OSPF Area Types
*Multiple areas are used to shrink LSDB size and isolate troubles

	
  * Backbone Area (Area 0) - Distributes routes between different areas

	
  * Stub Area - No AS External LSAs (Type 4 & 5) are flooded, ABR floods default route

	
  * Totally Stubby Area - Receives only default route (No Type 3, 4, or 5)

	
  * Not-So-Stubby-Area - Can receive and leak external routes in area, but external routes from other areas are not flooded (No Type 4 & 5)


OSPF Route Types

	
  * External - Redistributed from other protocols (LSA Type 5 & 7)

	
  * Inter-Area - (Summary Routes) Originate from other areas (LSA Type 3 & 4)

	
  * Intra-Area - (Internal Routes) Originate within same area (LSA Type 1 & 2)


OSPF LSA Types

	
  * Type 1 - Router - Intra-Area Adjacencies (DROther to DR)

	
  * Type 2 - Network - Describe Broadcast Segment (DR to DROthers)

	
  * Type 3 - Summary - Area Summary Sent by ABR to other areas
*OSPFv3 - Inter-Area-Prefix-LSA


	
    * As re-injected into other areas LSA type doesn't change


	
  * Type 4 - ASBR Summary - ASBR Description Sent by ABR to other areas
*OSPFv3 - Inter-Area-Router-LSA


	
    * As re-injected into other areas LSA type doesn't change


	
  * Type 5 - AS-External - Sent by ASBR describing prefixes from other protocols
*OSPFv3 - AS-External-LSA


	
    * As re-injected into other areas LSA type doesn't change


	
  * Type 6 - Used by Multicast OSPF, deprecated in OSPFv3

	
  * Type 7 - NSSA External - Sent by ASBRs in NSSAs


	
    * Translated to Type 5 LSA by NSSA ABR


	
  * Type 8 - External Attributes (Intended to mimic capability of iBGP)
*OSPFv3 - Link-LSA

	
  * Type 9 - Opaque LSA (Link Scope)
*OSPFv3 - Intra-Area-Prefix-LSA

	
  * Type 10 - Opaque LSA (Area Scope)

	
  * Type 11 - Opaque LSA (AS Scope)


