---
author: robertjuric
comments: true
date: 2010-02-13 01:45:49+00:00
layout: post
slug: stp-configuration
title: STP Configuration
wordpress_id: 49
categories:
- Networking
tags:
- Cisco
- STP
- Switching
---

This is a brief overview of the commands necessary to properly setup STP

1. Set STP Mode
_(config)# spanning-tree mode rapid-pvst_

2. Influence STP Topology - Specify Root and Secondary Switches
_(config)# spanning-tree vlan __vlan-id root primary_
Tells the switch to use pick a priority value, for the specified VLAN, that will force the switch to become the root switch. The same command can be given as 'root secondary' to specify the backup switch. You can also use _spanning-tree vlan <vlan-id> priority <#> _command to modify the base priority, however it is preferred to specify the root switch.

3. Influence STP Topology - Influence RP Selection
If you have two links to the root switch, port cost will decide which becomes the root port. Since port cost is by default based on bandwidth, when there is a tie it is decided by the lowest MAC address. You can influence this decision by manually lowering the port cost on the port you would like to be used as the root port:
_(config-if)# spanning-tree vlan <vlan-id> cost <#>_

*When setting the root switch or port costs you can either use the 'vlan _vlan-id_' command or omit it and apply the changes to all VLANs.

4. Enable Other Features
On switch ports that will only be used as access ports it is best to configure PortFast and BPDU Guard:
_(config-if)# spanning-tree portfast
(config-if)# spanning-tree bpdu guard enable
_You can_ _also specify BPDU guard to be on by default any time PortFast is enabled with the global command:
_(config)# spanning-tree portfast bpduguard default_
When BPDU guard finds a problem it will disable the port until you manually reset it. To have the port reenable itself use errdisable recovery:
_(config)# errdisable recovery cause bpduguard_
The default timeout is 300 seconds, you can modify this setting with:
_(config)# errdisable recovery interval <###>_

Below are some default settings and the commands to modify them:
<table border="1" >
<tbody >
<tr >

<td >Setting
</td>

<td >Default
</td>

<td >Commands
</td>
</tr>
<tr >

<td >Bridge ID
</td>

<td >Priority + MAC
</td>

<td >spanning-tree vlan _vlan-id_ root {primary | secondary}
spanning-tree vlan _vlan-id_ priority _priority_
</td>
</tr>
<tr >

<td >Interface Cost
</td>

<td >100Mbps=19, 1Gbps=4, 10Gbps=2
</td>

<td >spanning-tree vlan _vlan-id_ cost _cost_
</td>
</tr>
<tr >

<td >Port Fast
</td>

<td >Disabled
</td>

<td >spanning-tree portfast
</td>
</tr>
<tr >

<td >BPDU Guard
</td>

<td >Disabled
</td>

<td >spanning-tree bpduguard enable
</td>
</tr>
</tbody>
</table>
To alleviate some of the need for STP, but still retain port failure high availability you can configure ports in EtherChannels with multiple interfaces added to a port channel:
_(config-if)# channel-group <group#> mode on_

And finally some helpful debug and show commands:
_show spanning-tree vlan <vlan-id>__
show spanning-tree summary totals
show etherchannel <#> summary
debug spanning-tree events_

Feel free to comment any suggestions or corrections!
