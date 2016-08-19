---
author: robertjuric
comments: true
date: 2011-12-21 03:12:55+00:00
layout: post
slug: perl-reading-from-file
title: Perl - Reading from File
wordpress_id: 587
categories:
- Code
tags:
- file
- input
- Perl
- Ping
---

In my [first attempt at Perl scripting](http://robertjuric.com/2011/12/18/perl-beginnings/) I wrote a short script to ping a host taken from a command-line argument. I now want to expound on that idea by pinging a list of hosts saved in a text file. To do this we simply open the text file as an array of lines and then use a "for...each" loop to ping each host individually. Each line contains a hidden newline character which has to be removed in order for the Net::Ping method to process the variable correctly. I attempted to use the Perl Chomp() function, however this was yielding strange results. After some experimentation I found an answer using my first foray into regular expressions (regex). Using the Perl search and replace regex function I was able to search the individual line for the newline character and replace it with nothing. I used a handy little tool, which you can find [here](http://www.regextester.com/), to practice and see if my regex matched correctly on the newline character. 

Here is the script I ended up with:

    
    
    use Net::Ping;
    use strict;
    open(MYINPUTFILE, "hosts.txt");
    my(@hosts) = <MYINPUTFILE>;
    my($host);
    foreach $host (@hosts) {
    	$host =~ s/s+$//; #strip newline character from variable
    	my $pingobj = Net::Ping->new('icmp');
    	my ($status,$time,$ip) = $pingobj->ping($host);
    	if ($status) {
    		print "Host $host ($ip) responded in $time secondsn";
    	} else {
    		print "Host $host ($ip) unreachablen";
    	}
    }
    close(MYINPUTFILE);
    



And the results:
[![](http://robertj.files.wordpress.com/2011/12/screenshot017.jpg)](http://robertj.files.wordpress.com/2011/12/screenshot017.jpg)

I created a hosts.txt file and put it alongside the script and for testing purposes I tried a few different formats for host entries as well as a "null" value to make sure the script was working as designed. I hope you can pick up where I am headed with this building block. This script will be the basis for more advanced scripts such as automating changes to multiple devices. 
