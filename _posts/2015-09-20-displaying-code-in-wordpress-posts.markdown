---
author: admin
comments: true
date: 2015-09-20 15:44:58+00:00
layout: post
slug: displaying-code-in-wordpress-posts
title: Displaying Code in Wordpress Posts
wordpress_id: 1214
categories:
- Wordpress
tags:
- code
- format
- formatting
- post
- post format
- wordpress
- wordpress.org
---

_Last verified as of Wordpress 4.3.1._

I'm going to show how I use different Wordpress formatting to relay segments of code or commands in my posts. I normally write my posts using the plain 'Text' edit mode because I enjoy the simplicity and control.

The first and least special way I like to relay code or command syntax information in my posts is to switch to the Visual edit mode and then use the 'Italic' and 'Increase indent' formats.
[![Toolbar](http://robertjuric.com/wp-content/uploads/2015/09/Toolbar.jpg)](http://robertjuric.com/wp-content/uploads/2015/09/Toolbar.jpg)

Which produces results like this:


_router# configure terminal
router(conf)# interface gi0/0
router(conf-if)# ip address 192.168.1.1 255.255.255.0_


The other format that I like to use when I want to highlight commands within a paragraph. I do this in 'Text' edit mode and specify the <code> </code> tags around the text. Doing so lets me `highlight a section of code` within a sentence to signify that it is a commnd or piece of code.

If I want to set aside an entire block of code with multiple lines I like to use the <pre> </pre> tags in addition to the <code> tags.

    
    <code>This allows
    me to use multiple
    lines of code
    grouped together</code>


This is my favorite way of displaying blocks of information because it emphasizes its importance and keeps multiple lines together.

The last bit of advice I would offer is that sometimes when I display a command syntax there are "variable" components to the command such as an interface name or a description. When I display something as variable I like to surround it with the < and > symbols. However since Wordpress is HTML based it assumes that text is an HTML tag and won't display it correctly. To overcome this you have the replace the "<" symbol with the text "&lt" followed by a semicolon, without the space: & l t ;. The same goes for the ">" symbol but using gt instead of lt.

    
    <code>router(conf)# interface <name>
    router(conf-if)# description <your description>
    </code>

That's all I've got for now. Knowledge is power.
