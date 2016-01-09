---
author: robertjuric
comments: true
date: 2011-11-22 14:23:44+00:00
layout: post
slug: working-with-ipv6-and-juniper-srx
title: Working with IPv6 and Juniper SRX
wordpress_id: 540
categories:
- Juniper
- Networking
tags:
- 6to4
- IPv6
- Juniper
- JUNOS
- slaac
- SRX
- SRX100
---

I finally decided that if I don’t run IPv6 then I am never truly going to understand it. I figured the best place to get started would be my own home. I didn’t want to completely get rid of my IPv4 network, I only wanted to try to get IPv6 up and running, so I decided to run a dual-stack. Since the computers are running Windows 7 and I have an SRX100 as our household firewall this proved not to be a problem. Also, it was only an after-thought, but apparently our wireless access point would pass the IPv6 traffic without any problems. My next problem was external IPv6 connectivity, what good would my private IPv6 network be if I couldn’t connect to any other IPv6 networks?

Enter Hurricane Electric and their free IPv6 tunnel broker service at [http://tunnelbroker.net/](http://tunnelbroker.net/). This is a free service that allows you to build a 6to4 tunnel to their network which then acts as your IPv6 gateway. 6to4 tunneling (RFC 3056 [http://tools.ietf.org/html/rfc3056](http://tools.ietf.org/html/rfc3056)) is an IPv6 transition mechanism which allows IPv6 packets to be encapsulated (aka tunneled) within IPv4 packets to a 6to4 gateway router which is then de-encapsulated and sent on to IPv6 networks. So I registered with HE using my external IPv4 address as the tunnel end-point and was assigned my tunnel endpoints as well as my very own routed /64 block of IPv6 addresses. I was also given an IPv6 DNS server to point to. It was finally time to get IPv6 up and running.

The first thing I needed to do was get my tunnel up and running. On my SRX100 this involved creating an IP tunnel interface, adding an IPv6 static route, and putting the SRX in packet-mode for IPv6 traffic.

    
    
    interfaces {
       ip-0/0/0 {
          unit 0 {
             tunnel {
                source 99.111.88.228;
                destination 216.66.22.2;
             }
             family inet6 {
                address 2001:470:7:e9e::2/64;
             }
          }
       }
    }
    routing-options {
       rib inet6.0 {
          static {
             route ::/0 next-hop 2001:470:7:e9e::1;
          }
       }
    }
    security {
       forwarding-options {
          family {
             inet6 {
                mode packet-based;
             }
          }
       }
    }
    


At that point I could see my tunnel was operational and I was able to ping the remote end of the tunnel. Next I needed to get IPv6 running internally. I decided to use Stateless Address Auto Configuration (SLAAC) for address configuration. Devices running IPv6 will automatically configure for themselves a link-local address. This address is created using the FE80::/64 block + the EIU64 Interface ID. The EIU64 Interface ID is dynamically created using the Ethernet MAC address. The link-local address is required for basic IPv6 operations such as Neighbor Discovery Protocol (NDP) which is used for SLAAC. Basically the link-local address allows for local communication of hosts without configuration in order to facilitate higher-level communications. One thing about link-local address are that they are not globally routable, which means that my computers with only a link-local IPv6 address would not be able to communicate with other IPv6 devices such as web servers.

I needed to use my /64 block of globally routable IPv6 addresses I was given and assign them to my hosts. To do this I needed to configure IPv6 Router Advertisements (RAs) to allow my hosts to use SLAAC and auto-configure themselves an IPv6 address.

    
    
    interfaces {
       vlan {
          unit 0 {
             family inet {
                address 192.168.1.1/24;
             }
             family inet6 {
                address 2001:470:8:e9e::1/64;
             }
          }
       }
    }
    protocols {
       router-advertisement {
          interface vlan.0 {
             max-advertisement-interval 20;
             min-advertisement-interval 15;
             prefix 2001:470:8:e9e::/64;
          }
       }
    }
    


Now my hosts have both a link-local and a global IPv6 address and are able to ping through my tunnel. So far I haven’t done enough experimenting to get DHCPv6 working for DNS server assignment so I currently have my hosts DNS servers statically configured. I was then able to go online and check my IPv6 connectivity via [http://test-ipv6.com/](http://test-ipv6.com/).

Caveats:
Packet-based forwarding of IPv6 - Still researching the implications of this in regards to security
Had to use Junos 10.4R7.5 to assign family inet6 on vlan interfaces
Still have to experiment with DNS server assignment either through DHCPv6 or some other means
Lack of IPv6 websites/stuff to do
Security Considerations for 6to4 RFC 3964 [http://tools.ietf.org/html/rfc3964](http://tools.ietf.org/html/rfc3964)
