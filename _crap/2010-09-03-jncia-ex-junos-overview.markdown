---
author: robertjuric
comments: true
date: 2010-09-03 02:48:06+00:00
layout: post
slug: jncia-ex-junos-overview
title: JNCIA-EX / Junos Overview
wordpress_id: 157
categories:
- Juniper
- Networking
tags:
- JNCIA
- Juniper
- JUNOS
---

I've decided to take a break from my CCNP studies to pursue the JNCIA-EX certification. I've scheduled my exam for the end of September, so I've got some serious studying to do. I decided that an overview of Juniper's Junos CLI would be a good first Juniper blog post.

**CLI**
JUNOS runs on a modified BSD kernel, and because of this when you first log into a device as root you are at the BSD shell. Like Cisco there are different CLI prompt symbols to let you know what level you are at. '%' Specifies the BSD shell. From the BSD shell you can run the command 'cli' to start the Junos CLI in Operational Mode, where you will receive the '>' prompt symbol. From op mode you can type 'configure' to enter Configuration Mode where you will see the '#' prompt symbol. Most of the commands available in operational mode include the 'show' commands, as well as ping, traceroute, telnet, and ssh. To run an operational mode command in config mode you simply preface the command with 'run'. Unlike Cisco's IOS 'do' command, Juniper's JUNOS 'run' command can be used in any config mode context.

**Configs**
The JUNOS config uses a hierarchical design much like XML. Throughout the hierarchy you  can use 'set', 'delete', or 'rename' to modify config parameters. With JUNOS, unlike IOS, when you enter configuration mode your changes do not take immediate effect. Instead, when you enter configuration mode, you are given a candidate configuration. The candidate configuration is an exact copy of the active configuration, but allows you to make changes without affecting the active configuration. After changes have been made to the candidate config, and you wish to apply the changes to the active config you must save the changes with the 'commit' command. JUNOS also has a rollback configuration which is automatically updated with each commit. JUNOS stores the 50 most recent configs directly on the device, rollback 0 (the active config) and 49 saved rollback configs.

So for a little recap, the device is running the Active configuration. When you enter config mode, you are give a Candidate configuration. After you save the changes with the 'commit' command, the candidate configuration becomes current Active configuration. The previous active configuration becomes Rollback 1, and all the other rollback configs increment down the line. Rollback configs 1-3 are stored on the device in /config/. Rollback configs 4-49 are stored in /var/db/config/.

If you mess up working on the candidate config and want to start again with a fresh candidate config you can use the command 'rollback 0'. If you decide you need to roll back your active config you are able to load any of the 49 stored rollback configs with the command 'rollback _n_'.

If you want to save a snippet of config you can use the 'save' command. The save command only works at the current config hierarchy, so if you want to save the entire config, you need to make sure you're at the top of the config. This saved config is stored in the user's home directory on the device, unless a remote FTP or SCP server is specified.

If you want to load a snippet you have two options, you can use 'load override' or 'load merge'. 'Load override' will replace the candidate configuration with the loaded config. 'Load merge' will add the loaded config snippet to the candidate configuration without replacing anything. If you want to paste a config snippet through the terminal you can use 'load merge terminal' command and then paste the snippet. If you have a snippet that includes the set commands you will have to use 'load set terminal'.
