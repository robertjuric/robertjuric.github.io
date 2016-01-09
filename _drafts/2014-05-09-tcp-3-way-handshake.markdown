---
author: admin
comments: true
date: 2014-05-09 17:45:36+00:00
layout: post
slug: tcp-3-way-handshake
title: TCP 3-Way Handshake
wordpress_id: 975
categories:
- Networking
tags:
- handshake
- Network
- tcp
---

Dude 1: Hey, can we talk?
Dude 2: Yea, we can talk.
Dude 1: Cool.


<blockquote>If you can't explain it simply, you don't understand it well enough.
-Albert Einstein</blockquote>


But in all technical seriousness, let's talk about TCP. I think it's important for network engineers to understand TCP because it is one of the 2 protocols used for transport by higher level protocols such as HTTP, SSH, FTP, etc. Understanding that transport protocol can help when supporting other teams in your environment. Sometimes saying, "well I can ping the server, it's not a network problem" doesn't cut it, and that just encourages finger pointing. So after we have verified Layer 3 connectivity, we can start looking at troubleshooting Layer 4. This is usually best done with a packet capture, and where a good understanding of TCP can help.

If we break down troubleshooting Layer 4, and specifically TCP, the first thing that we need to verify is whether or not TCP is establishing 2-way communication between the end-hosts. TCP uses the famous 3-way handshake to establish this communication.

`SYN
SYN-ACK
ACK`

At the end of a successful handshake, a socket has been created and is used to transmit data. If there is a problem completing the handshake then the 2-way communication needs to be verified. TCP uses a similar, but 4-way, handshake to tear-down connections once the data transmission is complete:

Dude 1: Hey, I've got to run.
Dude 2: Ok, that's cool.
Dude 2: I'll see you later.
Dude 1: Bye.

We have a 4-way because both ends have to request (`FIN-ACK`) and agree (`ACK`) to tear down the connection. There are ways to cheat and end the connection sooner using `RST` flags, but that's not always ideal. So that's just a little about starting and tearing down TCP sessions, which can be helpful in troubleshooting TCP connectivity between end-hosts. In a future post I plan on writing a little more on the other TCP flags and maybe a post about performance issues.
