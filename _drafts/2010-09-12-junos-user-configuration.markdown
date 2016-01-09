---
author: robertjuric
comments: true
date: 2010-09-12 02:47:18+00:00
layout: post
slug: junos-user-configuration
title: JUNOS User Configuration
wordpress_id: 169
categories:
- Juniper
- Networking
tags:
- CLI
- config
- Juniper
- JUNOS
---

Continuing with my Juniper posts, I was going to write a larger post covering a few topics but once I got started writing I realized how large a post this grew to be. So with this post I will only cover some basic user setup to get you started using a Juniper device.

Before you can complete your first commit on a Juniper device you must set a root user password. You can use the following command to set the root users password: `set system root-authentication plain-text-password.` You will then be prompted to enter and confirm the password. It is important to note that the root user is the only predefined user on JUNOS, and this user also can only log in via the console and SSH, the root user is not allowed to telnet to the device.

After configuring the root user you may want to create individual user accounts. The process to do this is fairly simple, you create the user, assign it to a user class, and then set a local password. The user class defines the privilege and access level. JUNOS includes 3 predefined user classes: _operator_ which allows clear, network, reset, trace, and view commands,_ read-only_ which allows for view commands, and _super-user_ which allows all commands. If you need more granular control, you can create custom user classes. To create a new user account you can use the following command: `set system login user <username> class <user-class> authentication plaint-text-password.` You will then be prompted to enter and confirm the password. (I know I'm skipping RADIUS and TACACS, but I will come back to those later.)

If you happen to forget or need to reset the root password this is a pretty simple process. You need to console to the device and reboot it. Pay attention to look for the prompt to press [Spacebar] for a command prompt. Press the space bar and the boot process will be interrupted and you will be given a `loader>` prompt. Type `boot -s` to enter single-user mode. Once it finishes loading type `recovery` and you will finally arrive at the CLI. From here you can enter config mode, set the root-authentication password again, commit the change, and then reboot the switch with the `request system reboot` command.
