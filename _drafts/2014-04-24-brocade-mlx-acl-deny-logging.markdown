---
author: admin
comments: true
date: 2014-04-24 20:33:03+00:00
layout: post
slug: brocade-mlx-acl-deny-logging
title: Brocade MLX ACL Deny Logging
wordpress_id: 972
categories:
- Networking
tags:
- ACL
- Brocade
- logging
- MLX
- MLXe
---

I just spent 3 hours troubleshooting an ACL, well let me restate... I just spent 3 hours TRYING to troubleshoot an ACL on a Brocade MLX. My ACL had a `deny ip any any log` at the end. However, the logs were not showing anything, nothing, 'ACL Applied...ACL Removed...'. Very frustrating.

Anyway, after much Googling, deep in some obscure documentation I found out that there is a separate command which must be applied to the interface to enable the logging (which was already enabled in the ACL).

`ip access-group 199 in
ip access-group enable-deny-logging`

Some things that 'just work' on Cisco really make me scratch my head when working on Brocade equipment. Lesson learned, let's move on. Maybe this post and the tags will help some future Googler.
