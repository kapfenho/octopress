---
layout: post
title: "Cisco VPN with Mac OS X client - problems and extensions"
date: 2011-12-28 21:54
comments: true
categories: 
---
Mac OS X 10.7 Lion - as other versions before - has a built in VPN client for Cisco VPN (IPSec). This could well be an alternative to 3rd party software like the Cisco client for Mac, or the open source vpnc package. But there are some pitfalls too.

<a title="Setup Cisco VPN on Mac OS X" href="http://anders.com/guides/native-cisco-vpn-on-mac-os-x/" target="_blank">Here is how to setup or migrate your Cisco VPN connection</a>. Thanks to Anders Brownworth for this article.

However you might have to fix some problems until you can enjoy your stable connectivity.

One common problem is that the client drops the connections after 48 to 60 minutes, at the time it should exchange a new phase one key pair.  Simon Heimlicher has a <a title="Work around for Cisco VPN disconnection on Mac OS X" href="http://simon.heimlicher.com/articles/2011/03/17/cisco-vpn-10.6.0-3/" target="_blank">well documented work-around</a> for that problem.

Another intended behavior may become a problem when the Cisco VPN profile tells the client not to allow local network connections.  This is basically done by the clients routing table, so let's change it.  This you do after the VPN connection has been established.  You have to do this after every login, so it's better you create a script that  you can execute from your home directory or better from ~/.scripts.

First you have to know what is your local network, in this example this is 192.168.0.0/16 (/16 that means from 192.168.0.0 to 192.168.255.255, <a title="Wikipedia: reserved IP addresses" href="http://en.wikipedia.org/wiki/Reserved_IP_addresses#Reserved_IPv4_addresses" target="_blank">explanation</a>).  Then you need to know the company network(s). Ok, here we have e.g. 10.0.0.0/8, 172.16.0.0/12, and a public one like 107.94.64.0/18.

Here are the necessary steps, you can create a script with them that you need to run with <a title="Sudo" href="http://en.wikipedia.org/wiki/Sudo" target="_blank">sudo</a>.

``` bash Reset VPN routing
#!/bin/sh
# run this script with sudo (sudo script.sh)
# route company networks over vpn
TUNNEL=`netstat -rn -f inet | grep -o utun[0-9] | uniq`
route add 10.0.0.0/8 -interface $TUNNEL
route add 172.16.0.0/12 -interface $TUNNEL
# sample: route add 107.94.64.0/18 -interface $TUNNEL
# now delete the default route for vpn
route delete default -interface $TUNNEL
# and reactivate your own default route
# default route needed without flag I (interface)
route add default 192.168.1.1
```

After that you are able to reach again your home network and all internet traffic will run over your default route.
