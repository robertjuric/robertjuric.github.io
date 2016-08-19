---
author: robertjuric
comments: true
date: 2010-02-11 04:04:28+00:00
layout: post
slug: stp-notes-continued
title: STP Notes Continued
wordpress_id: 57
categories:
- Networking
---

STP, RSTP, PVST+, RPVST

STP (802.1d) - Spanning Tree Protocol - Covered [Here](http://robertj.wordpress.com/2010/02/04/stp-notes/)

RSTP (802.1w) - Rapid STP
Improves convergence times by reducing waiting times when reacting to changes in topology. Renames port states to Discarding, Learning, and Forwarding. Implements a feature to characterize connectivity methods called either Link-type (Switch-to-Switch) or Edge-type(Switch-to-Device). Adds additional port roles to include Root Port, Designated Port, Alternate Port (like a backup RP), Backup Port (when 2 links exist in the same collision domain), and Disabled port (shutdown ports). In RSTP, instead of waiting to forward Hello BPDUs from the root switch, switches independently create Hello BPDUs.

PVST+, PVRST or RPVST
Allows creation of spanning trees for each VLAN. By doing so you are able to load-balance VLANs across trunks by setting different port costs for the different VLAN spanning-trees. The ability to create separate spanning trees for each VLAN is made possible by extending the Bridge ID. As I noted earlier the Bridge ID is made up of a priority and part of the MAC address. PVST and RPVST go a step further and extend the priority field into a smaller priority field and a System ID field which holds the VLAN ID. This will create a different BID for each VLAN.
