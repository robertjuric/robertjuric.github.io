---
author: robertjuric
comments: true
date: 2010-07-01 16:19:22+00:00
layout: post
slug: data-center-move
title: Data Center Move
wordpress_id: 129
categories:
- Insights
- Networking
---

I was trying to think of my next blog post and I was running out of ideas when it hit me that we're moving data centers! Not only are we moving data centers, we are collapsing two data centers into a single data center through virtualization. We are also spinning off a division of our company as a standalone entity with it's own infrastructure.

To bring you up to speed, we are breaking this into 4 large projects: 1. Setting up the new data center infrastructure for both companies. 2. Moving the spin-off company to their new data center. 3. Collapsing/moving the Southern data center to the new data center, and then finally 4. Moving the main data center. We currently have about 70 physical servers and 70 virtual servers. We are a VMWare shop and plan on either P2V'ing the physical servers or just rebuilding them as virtual and migrating data, when all is said and one we are expecting to have around 200-250 virtual machines and are not expecting to run into any machines that we can't virtualize.

We started out with the idea of building out our own data center, but when we got down to engineering it out at our location, cooling costs were going to blow our budget. Also, because we are a relatively small IT department we weren't too excited about having to manage the cooling, power, generators, etc. which eventually led us to look for hosted racks at a larger provider.

So far we have been spending most of this year researching vendors and trying to design our brand-new data center. We have been researching SANs, switches, backup solutions, VMWare, servers. So much stuff in such a short period of time that my head hurts at times. But what a great learning experience, one that I hope to share with you as we progress through this project.
