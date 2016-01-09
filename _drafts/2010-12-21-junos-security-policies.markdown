---
author: robertjuric
comments: true
date: 2010-12-21 17:00:36+00:00
layout: post
slug: junos-security-policies
title: JUNOS - Security Policies
wordpress_id: 317
categories:
- Juniper
- Networking
tags:
- Juniper
- JUNOS
- policy
- security
---

As I have been continuing to setup our Juniper SRX firewalls I thought I would take a moment to discuss how to implement security policies in JUNOS. Before we get started I think it's important to cover a few quick concepts:



	
  * Stateful Firewall - Monitors the state of network connection streams so that when traffic is inspected and permitted in one direction, the return traffic is also permitted.

	
  * Zone-Based Firewall - Interfaces are arranged into groups called zones to build security boundaries

	
  * JUNOS Security Policy Applications - Applications are a sort of profiles for apps which have defined protocol standards. Applications store information such as transport protocol, source and destination port numbers, and timeout values. These applications can be grouped into an Application-Set in order to simplify policy creation for multi-port applications. JUNOS has many predefined applications and also has the capability to define custom applications as needed. For more information, check out [Juniper's documentation](http://www.juniper.net/techpubs/software/junos-security/junos-security10.0/junos-security-swconfig-security/policy-apps-understanding.html#policy-apps-understanding).

	
  * JUNOS Security Addresses - When building security policies, JUNOS relies on predefined addresses or address books which contain entries for either IP prefixes or FQDNs. Multiple addresses can be grouped together into an address-set to simplify policy creation for a group of address or prefixes.


JUNOS security policies are different from [firewall filters](http://robertjuric.com/2010/09/19/junos-firewall-filters/) in that security policies are stateful and applied to security zones, while firewall filters are stateless and applied directly to interfaces. Now let's get to it. The first thing we have to do is setup our security zones. A security zone is configured with 3 main components, the address-book, the interfaces, and host-inbound-traffic rules. The first two are pretty self-explanatory; host-inbound-traffic rules are configured to secure traffic originating from connected hosts within a zone. In other words, host-inbound-traffic rules are used to protect a zone from internal attacks. See [here](http://www.juniper.net/techpubs/software/junos-es/junos-es92/junos-es-swconfig-security/configuring-host-inbound-traffic.html) for more information on configuring host-inbound-traffic.

Here is an example config for a trust and untrust zone:

    
    set security zones security-zone trust address-book address mail01 dns-name mail01srv.domain.com
    set security zones security-zone trust address-book address mail02 dns-name mail02srv.domain.com
    set security zones security-zone trust address-book address-set mail_servers address mail01
    set security zones security-zone trust address-book address-set mail_servers address mail02
    set security zones security-zone trust host-inbound-traffic system-services all
    set security zones security-zone trust host-inbound-traffic protocols all
    set security zones security-zone trust interfaces ge-0/0/0
    set security zones security-zone trust interfaces lo0.0
    
    set security zones security-zone untrust host-inbound-traffic system-services ping
    set security zones security-zone untrust host-inbound-traffic system-services traceroute
    set security zones security-zone untrust interfaces ge-0/0/1
    


Now that we have our two zones configured, they are by default policy not permitted to pass any traffic. So we must configure policies to allow traffic to flow between the zones. Security policies work much like firewall filters in that they use a series of "match...then" statements which are applied to traffic in the order they are listed. A policy 'match' statement must specify a source-address, a destination-address, and an application. These specifications can be address-sets and application-sets respectively. A policy 'then' statement can contain either:



	
  * Permit - Allows traffic to pass

	
  * Deny - Denies traffic to pass by *silently* dropping the packet

	
  * Reject - Denies traffic to pass by dropping the packet, but also sends an ICMP Port Unreachable reply for every dropped packet

	
  * Log - An option to enable logging of the sessions, requires the creation of a logfile via "set system syslog file <file-name> <options> "

	
  * Count - An option to enable counting (in bytes or kilobytes) of all traffic permitted by the policy


Here is an example of a few policies which will first allow SMTP traffic from my mail servers, then block SMTP from any host, and finally all any traffic:

    
    set security policies from-zone trust to-zone untrust policy allow-smtp match source-address mail_servers
    set security policies from-zone trust to-zone untrust policy allow-smtp match destination-address any
    set security policies from-zone trust to-zone untrust policy allow-smtp match application junos-smtp
    set security policies from-zone trust to-zone untrust policy allow-smtp then permit
    
    set security policies from-zone trust to-zone untrust policy block-smtp match source-address any
    set security policies from-zone trust to-zone untrust policy block-smtp match destination-address any
    set security policies from-zone trust to-zone untrust policy block-smtp match application junos-smtp
    set security policies from-zone trust to-zone untrust policy block-smtp then deny
    set security policies from-zone trust to-zone untrust policy block-smtp then log session-init session-close
    
    set security policies from-zone trust to-zone untrust policy internet-allow match source-address any
    set security policies from-zone trust to-zone untrust policy internet-allow match destination-address any
    set security policies from-zone trust to-zone untrust policy internet-allow match application any
    set security policies from-zone trust to-zone untrust policy internet-allow then permit
    


It is important to take note of the policy ordering. Security policies are applied in order. If my internet-allow policy were at the top of the list, it would make my SMTP filtering policies useless. I could fix this easily this with the command:

    
    insert policy <name>  {before/after} policy <name>


That gives you the basic run-down to configure security policies in JUNOS. I would recommend reading the documentation when it comes planning time so that you can properly configure the correct features for your requirements.
