---
author: robertjuric
comments: true
date: 2011-10-18 02:44:55+00:00
layout: post
slug: srx-firewall-user-authentication
title: SRX Firewall User Authentication
wordpress_id: 521
categories:
- Juniper
- Networking
tags:
- authentication
- firewall
- firewall-authentication
- firewall-user-authentication
- Juniper
- JUNOS
- SRX
---

Juniper SRX firewalls have a feature called Firewall User Authentication. FUA is another layer of protection on top of security policies to restrict/permit users or groups of users individually. FUA is a very limited feature but it could be used as a poor-man's NAC in a pinch. Users are authenticated via a single external source(RADIUS, LDAP, or SecureID) or the local database. FUA can operate in two different modes, Pass-Through Authentication, and Web Authentication.

Pass-Through Authentication is limited to Telnet, FTP, and HTTP traffic only(notice these are plain-text protocols). Pass-through auth works by interjecting a login challenge into the original protocol login challenge. For example a user would initiate a telnet connection, the FUA would interject and prompt the security challenge. Once authenticated, traffic from the same source IP is automatically allowed to pass through the SRX(could be dangerous with source NAT). This would allow the telnet connection to continue and the user can authenticate to the telnet host. Once the user has authenticated via FUA the IP address becomes exempt from further FUA requirements. This means that you can create a security policy that matches on ANY traffic type and require FUA; Pass-through authentication only works with Telnet, FTP, or HTTP, however once the IP/user has authenticated then it is exempt from further FUA, and it is then only bound by security policy. I said that to say this: you can use a telnet session to authenticate a user via FUA who, once authenticated, can pass any type of traffic which is allowed by the security policy.

Web Authentication operates in two sub-modes, standard web authentication, and web redirect. Standard web auth first requires you enable the HTTP system service, and also allow http in host-inbound-traffic. With standard web auth, you would direct your users to a secondary IP address of an interface on the SRX. Users would be prompted with an HTTP login form for the FUA. Once authenticated the IP address is added to the exempt list and is able to pass through the firewall as much as security policy allows. The web redirect operates like standard web auth, but you don't have to hand out an IP address of your firewall or direct users to a separate login page. Web redirect works much like what you're used to in a hotel room; You open your browser and head to Google.com, and you are automagically redirected to a login page. Once you authenticate you are able to pass through the firewall as much as security policy allows.

With all the authentication methods you are also able to configure a default timeout for inactivity and login/success/failure banners. Web redirect + a login banner would be a great poor-man option for a Guest Wireless NAC. You could hand out the username and password to guests, and they would be prompted with the Acceptable Use Policy before logging in.

All FUA configuration follows the same basic principles: configure an access profile, configure the firewall authentication options, and finally call on the firewall authentication from security policy. (Good to know on an exam).

Configure the access profile (local user database):


    
    set access profile *profile-name* client *name* firewall-user password *pwd*



or (external authentication):


    
    set access profile *profile-name* authentication-order [radius password]
    set access profile *profile-name* radius-server *ip-address* secret *radius-secret*



Then configure the authentication type and options:


    
    set access firewall-authentication pass-through default-profile *profile-name*
    set access firewall-authentication pass-through [telnet/ftp/http] banner login "You must login"



or


    
    set interface *name* unit 0 family inet address *ip/mask* web-authentication http
    set access firewall-authentication web-authentication default-profile *profile-name*
    set access firewall-authentication web-authentication banner success "Login successful"



Finally set a permit action of firewall-authentication in the security policy. For pass-through use: "Permit firewall-authentication pass-through client-match *client-name*". For web auth use: "Permit firewall-authentication web-authentication client-match *client-name*". For web redirect use the same as pass-through but add a web-redirect argument: "Permit firewall-authentication pass-through web-redirect".

And finally to verify:


    
    show security firewall-authentication users
    show security firewall-authentication history
