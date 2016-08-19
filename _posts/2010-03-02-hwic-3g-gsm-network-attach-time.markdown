---
author: robertjuric
comments: true
date: 2010-03-02 02:44:17+00:00
layout: post
slug: hwic-3g-gsm-network-attach-time
title: HWIC-3G-GSM - Network Attach Time
wordpress_id: 78
categories:
- Networking
tags:
- 3G
- Cisco
- wwan
---

As of last week I now have two 3G cellular enabled routers running two different plants. One painful issue we experienced with our first installation was the long network attach time, or the time for the HWIC to get a signal. In the case of a power outage, or router reload, the time for the card to find a signal again was painfully slow. After a few incidents and much research I finally found in the Cisco documentation stating for North American installs you should run a one-time CHAT script to narrow the network search range to North American Bands only. I experimented with this CHAT script on my second install and saw a major improvement. I then went back and ran the script on my first installation with the same results. I feel that it is safe now to add to my list of documentation.

For the initial configuration notes see my original post [here](http://robertj.wordpress.com/2010/01/26/hwic-3g-gsm/).

_(config#) chat-script prl "" "at" TIMEOUT 5 "OK" AT!ENTERCND="A710" TIMEOUT 5 "OK" AT!CUSTOM="PRLREGION",02 TIMEOUT 5 "OK" "AT!RESET"_

_(config#) int cellular 0/0/0
(config-if#) shutdown
(config-if#) exit_

_(config#) debug chat_

_(config#) start-chat prl 0/0/0_

_(config#) int cellular 0/0/0
(config-if#) no shutdown
(config-if#) exit_

The first part of this snippet creates the script. We then shut down the cellular interface. Turn on CHAT debugging so we can verify that the script is applied successfully. Run the script, and after verification, bring the interface back up. Be sure to do a _'no debug all' _to turn off debugging.

Feel free to leave any feedback in the comments.
