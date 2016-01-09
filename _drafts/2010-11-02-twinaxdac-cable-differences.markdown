---
author: robertjuric
comments: true
date: 2010-11-02 18:07:16+00:00
layout: post
slug: twinaxdac-cable-differences
title: Twinax/DAC Cable Differences
wordpress_id: 285
categories:
- Insights
- Networking
tags:
- active
- cable
- DAC
- passive
- twinax
---

I am currently in the process of implementing Juniper EX4500 switches to deliver 10GE connectivity to our ESX host servers. During this process we decided to go the route of direct attach copper (DAC) cables to save money. We dug around what little research we could find and eventually decided to purchase Brocade converged network adapters (CNAs) to install in our servers. When we finally got around to mocking up this setup in our lab this week we ran into a major issue. I was unable to get the physical link between the server and the switch to come up. We swapped out the cable to try this Brocade DAC cable we just happened to have laying around and the link came right up. That was interesting, so we started doing a little research...

Are all DAC cables created equally? The answer is no. There are two types of DAC cables: Active and Passive. Scientific Computer has a [good article](http://www.scientificcomputing.com/High-speed-Copper-Interconnects-Address-Critical-HPCHurdles.aspx) listing the differences between active and passive cables.

[![](http://robertj.files.wordpress.com/2010/11/2010-11-02_12-28-36_491.jpg?w=300)](http://robertj.files.wordpress.com/2010/11/2010-11-02_12-28-36_491.jpg)You can see here three different cables. The one on the left is the Brocade active cable. The middle is the Juniper passive cable, and the one on the right is a Cisco passive cable. You can see the contact pad difference between the active and passive cables.

What does this mean to us network engineers? Vendor support. Not every vendor supports both passive and active cables, while some vendors require active cables. Take our situation for example: Juniper does not currently support active DAC cables, however the Brocade CNAs we purchased will only work with an active cable. This put us in a tight spot. Take this opportunity to learn from our mistake, check with your vendors regarding passive/active cable support if you are planning on implementing 10GE direct-attach copper cables.
