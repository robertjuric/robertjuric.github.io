---
author: robertjuric
comments: true
date: 2010-06-23 02:30:35+00:00
layout: post
slug: hwic-3g-cdma-v-notes
title: HWIC-3G-CDMA-V Notes
wordpress_id: 121
categories:
- Cisco
- Networking
tags:
- 3G
- CDMA
- Cisco
- HWIC-3G-CDMA-V
---

I deployed our first 3G enabled router on the Verizon network last week. Configuring the CDMA HWIC is similar to configuring the GSM, except for the fact that I had to activate the modem first. With the GSM HWIC I was able to insert an already active SIM card and start running. The CDMA does not use a SIM card and has to use either OTA (Over The Air) or manual activation. To activate the CDMA modem I first ran a '_show cellular 0/0/0 radio_' to verify I had a signal and _'show cellular 0/0/0 network'_ to discover the SID (SystemID) and the NID (NetworkID) of the network.  The command to perform manual activation is:

_cellular slot/wic_slot/port cdma  activate manual mdn msid sid nid msl_

For the MDN and MSDIN I used the 10 digit telephone number. The SID and NID can be found in the show radio command and for the MSL I just used '0'.

Once activation was complete I was ready to roll. One thing I noticed was when I tried to run the chat script to reduce network attach time, the script failed out. I have not had any problems with network attach times with this CDMA HWIC. Also, because of my newly learned EasyVPN skills, no static IP addresses or static IP tunnels were necessary to get this site online. I have to say coupled with EasyVPN, this is starting to become a more easily deploy-able solution. I hope that the 3G technology can meet our business needs, as it would provide much greater flexibility

Edit/Update 7/27/2010:

I just attempted to activate another HWIC-3G-CDMA-V, but on a new IOS version, and I found out the command has changed somewhat. I was running Adv. Security 15.0(1)M2. The command was:

_cellular 0/0/0 activate manual mdn msid msl_

Again the MDN and the MSID are basically the 10-digit mobile number number and I used '0' for the Verizon MSL.
