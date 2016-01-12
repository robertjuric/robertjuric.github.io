---
author: robertjuric
comments: true
date: 2012-01-13 20:26:20+00:00
layout: post
slug: authenticating-cisco-voip-phones-via-dot1x
title: Authenticating Cisco VoIP Phones via Dot1x
wordpress_id: 602
categories:
- Cisco
- Networking
tags:
- authentication
- Cisco
- Dot1X
- Network Policy Server
- NPS
- VOIP
---

If anyone has attempted to authenticate Cisco VoIP phones via Dot1x and Microsoft Network Policy Server then you know there are a few small issues. First the username that the phones send to the Radius server exceeds the length allowed by Windows. Secondly it is not a straightforward setup to get the phones to properly authenticate. 

First in Windows Server 2008, Microsoft removed support for EAP-MD5 authentication. However you can re-add this support to NPS by modifying the registry according to [Microsoft KB922574](http://support.microsoft.com/kb/922574). 

Because phones are the exception to my normal radius policies, I used a separate Connection Request Policy for VOIP phones. I created one with the condition to match UserName = "CP-*". This should ensure that this policy only applies to Cisco phones(because all their usernames start with CP-*). 

Next we need to shorten the username which the phone sends so that we can match it to a valid Windows Domain Account. To do this we use the separate VOIP Connection Request Policy. Under Settings > Attributes we need to override the Username attribute using a match and replace. For example we find "CP-7975G-" and replace with "". This will essentially remove the first part of the username and leave us with the "SEP..." username. A separate match will have to be configured for every model of Cisco phone which needs to be authenticated.

Finally we need to configure the authentication methods. Under Settings > Authentication Methods we want to Override Network Policy Authentication Settings. We add the EAP Type "MD5-Challenge" and then check Encrypted Authentication (CHAP). This allows us to use a single Network Policy and override it when necessary using our VOIP Connection Request Policy. 

The only thing left to do is ensure that a Windows Domain Account is created and the Account Password is Stored using Reversible Encryption. Then enter the account password as the MD5 shared secret on the phone, turn Dot1x on the port and you're good to go. If you're using a Cisco switch, ensure you have dual-host authentication enabled so you can also authenticate a computer hanging off the phone.

If you need a little help troubleshooting NPS and can't find events in MS Event Viewer from CMD Prompt, Run As Admin, type: "auditpol /set /subcategory:"Network Policy Server" /success:enable /failure:enable". NPS logs are in Event Viewer/CustomViews/Server Roles/Network Policy Server
