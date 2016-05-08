---
author: robertjuric
comments: true
date: 2011-12-19 01:52:46+00:00
layout: post
slug: perl-beginnings
title: Perl Beginnings
wordpress_id: 576
categories:
- Code
- Networking
tags:
- Perl
- Ping
---

After reading some advice from some very smart people, I've decided to try my hand at learning a scripting language to aid in my network administration tasks. After some quick searching I've decided to learn Perl. From what I've gathered it has strong text manipulation capabilities which will come in handy parsing device configs or SNMP/SSH responses. I'm not going to attempt to teach programming basics, but what I hope to cover will be some useful examples and building blocks that we can use as network administrators.

For my scripting setup I will be focusing on a Windows perspective. Instead of using [ActivePerl](http://www.activestate.com/activeperl/downloads) I decided to use [Cygwin](http://www.cygwin.com/) and the Perl modules, so hopefully most of what I write will also apply to *nix users. I'm also using [Notepad++](http://notepad-plus-plus.org/) for my editing/coding, but you could also use the default Windows Notepad or if you want to be hardcore, Cygwin and VIM. I may include some VIM tips/shortcuts in future posts.

My first Perl script is not "Hello, World", but instead a simple Ping script. I used [Net::Ping](http://perldoc.perl.org/Net/Ping.html) to conduct a simple ICMP Ping and then output the results. The example I first used had a hard-coded variable for the host, which I left commented out for reference purposes. Instead, I added a line to receive a command line argument for the host variable.

    
    use Net::Ping;
    use strict;
    
    #Setting a variable
    #my $host = "www.google.com";
    
    #Using an arguement
    my $host = "$ARGV[$0]";
    my $pingobj = Net::Ping->new('icmp');
    my ($status,$time,$ip) = $pingobj->ping($host);
    
    if ($status) {
    print "Host $host ($ip) responded in $time secondsn";
    } else {
    print "Host $host ($ip) unreachablen";
    }
    


Voila!
[![](http://robertj.files.wordpress.com/2011/12/screenshot016.jpg)](http://robertj.files.wordpress.com/2011/12/screenshot016.jpg)

I plan on using this script as a building block to test connectivity before performing more complex tasks in the future. Hopefully this will motivate somebody to get their hands dirty and add another tool to their skill set.
