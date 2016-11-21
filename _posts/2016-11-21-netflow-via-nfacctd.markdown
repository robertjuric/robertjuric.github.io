---
layout: post
title: Netflow via nfacctd
categories: 
- Networking
--- 

I've been experimenting with with some network management systems at home and one piece that I keep getting hung up on is Netflow. I think there is great value in being able to see what type of traffic is leaving (or entering) your network, even at a macro scale. To test these systems I've been using my home Juniper SRX100 and various Open Source solutions to try to grab this netflow data and do something useful. I've discovered that grabbing the data isn't particulary difficult, but making it useful has been extremely difficult.  
  
I'm currently using nfacctd, from the [pmacct project](http://www.pmacct.net/). Within the pmacct project there is pmacctd which can grab traffic via libpcap and then aggregate it to make it useful for analysis. There is also nfacctd, which is the daemon that can listen for and process netflow/sFlow/IPFIX data. As far as my research has found the pmacct project is the most actively supported/developed open source project.  
  
It is quite simple to setup nfacctd however there are no front-ends for this data. There are a plethora of plugins available to export the data, everything from an in-memory database to MySQL, Kafka, or AMQP. I've tried setting some custom charts written in PHP for the MySQL data, but I'm not a developer and this turns out to be an inefficient use of my time. My latest attempt though has been to get the data into [InfluxDB](https://www.influxdata.com/) which I can then graph on using [Grafana](http://grafana.org/). So far this appears to be the most promising solution.  
  
As I complete stages in this project I plan on building documentation and sharing it via my [Projects page](http://robertjuric.com/projects/). So far I've done the base Debian install and the simple InfluxDB install. My next step is to create a bash script that will use the pmacct client and curl to send the data to the InfluxDB HTTP API. 