---
layout: post
title: Getting nfacctd to InfluxDB via Curl
comments: true
categories: 
- Networking
--- 

I wanted to highlight the process I used to get Netflow data gathered from nfacctd imported into InfluxDB. I found very little of this information online so I'm sharing it for my notes and if someone else needs it that's great also.

The first step in my process was to get some temporary data coming in from nfacctd. To do this I setup a config file to write to the memory plugin with debug mode enabled and daemonize disabled. My `nfacctd-test.conf` file looks like:  
  
```
!  
debug: true  
daemonize: false  
plugins: memory  
aggregate: src_host, dst_host, timestamp_start, timestamp_end, src_port, dst_port, proto, tos, tcpflags
nfacctd_port: 2100  
!
```
From there I opened another SSH session and used the pmacct client to see what my traffic would look like:
  
```
robert@debian-netflow:~$ pmacct -s -e
SRC_IP           DST_IP           SRC_PORT  DST_PORT  TCP_FLAGS  PROTOCOL    TOS    TIMESTAMP_START                TIMESTAMP_END                  PACKETS               BYTES
192.168.1.100    155.70.42.251    52579     443       26         tcp         0      2016-11-21 06:24:31.0          2016-11-22 04:23:25.0          6                     400
  
For a total of: 1 entries
```  
That line may be a little to wide to appear in the browser but it helped me to visualize what I was working with as I setup my awk statements later. I also made sure to use the `-e` flag with the pmacct client so I wouldn't import records multiple time into InfluxDB.

The first thing I did was use `sed` to remove the first and last lines, and then awk to print a test with some values:

```
pmacct -s -e | \
sed '1d;$d' | \
awk '{print "SRC_IP value="$1" DST_IP value="$2" BYTES value="$13}'
```
Which gave me a result of:

```
SRC_IP value=192.168.1.107 DST_IP value=8.8.8.8 BYTES value=90
SRC_IP value= DST_IP value= BYTES value=
``` 
So I added another iteration of `sed` to remove the blank line that was previously before the last line:

```
pmacct -s -e | \
sed '1d;$d' | \
sed '/^\s*$/d' | \
awk '{print "SRC_IP value="$1" DST_IP value="$2" BYTES value="$13}'
```
The next thing I wanted to do was pass my Netflow timestamp to InfluxDB. To do this I had to reformat the timestamp value using `gsub` and then convert that string into a Unix timestamp with the `mktime()` function of gawk. (I had to install gawk to make this work on my Debian machine).

```
pmacct -s -e | \
sed '1d;$d' | \
sed '/^\s*$/d' | \
awk '{gsub(/[-:]/, FS); print "traffic,src_ip="$1",dst_ip="$2" value="$21" "mktime($14" "$15" "$16" "$17" "$18" "$19)}'
```
The final step was to pass this again to curl to write to the InfluxDB HTTP API. I also went ahead and added the variables for the rest of the Netflow tags I'm capturing in nfacctd.  
When I first attempted this I noticed my timestamps were not taking correctly. After a little researched I discovered the `mktime()` function was creating a timestamp with seconds precision and InfluxDB was expecting a timestamp with nanosecond precision. So far I've just adjusted the curl statement to specify a precision of seconds:

```
pmacct -s -e | \
sed '1d;$d' | \
sed '/^\s*$/d' | \
awk '{gsub(/[-:]/, FS); print "traffic,src_ip="$1",dst_ip="$2",src_port="$3",dst_port="$4",tcp_flags="$5",proto="$6",tos="$7" value="$21" "mktime($14" "$15" "$16" "$17" "$18" "$19)}' | \
curl -i -XPOST 'http://192.168.1.73:8086/write?db=nfacctd&precision=s' --data-binary @-
```
So for now I'm going to pause with this setup to get some Grafana charts setup. I'll also some back to tweak my nfacctd script to run as a daemon and get the curl script running on a regular schedule. That's for another day.