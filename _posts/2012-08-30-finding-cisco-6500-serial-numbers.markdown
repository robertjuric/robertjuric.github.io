---
author: admin
comments: true
date: 2012-08-30 13:43:14+00:00
layout: post
slug: finding-cisco-6500-serial-numbers
title: Finding Cisco 6500 Serial Numbers
wordpress_id: 703
categories:
- Cisco
- Networking
tags:
- Cisco
- ios
- Serial Number
---

Learned something new today. To display the IDPROMs (serial numbers) for FRUs (Field Replaceable Units):


    
    show idprom {all | frutype | interface <interface> <slot>} [detail]



Some of the FRU types available: 
   backplane
   module <slot>
   rp
   power-supply
   supervisor

To find the chassis serial number use: 

    
    show idprom backplane



Will give the output similar to:

    
    6509#sh idprom backplane
    IDPROM for backplane #0
      (FRU is 'Catalyst 6500 9-slot backplane')
      OEM String = 'Cisco Systems'
      Product Number = 'WS-C6509'
      Serial Number = 'ABC12345678'
      Manufacturing Assembly Number = '12-3456-78'
      Manufacturing Assembly Revision = 'A0'
      Hardware Revision = 3.0
      Current supplied (+) or consumed (-) =  -



Definitely comes in handy for opening TAC cases.
