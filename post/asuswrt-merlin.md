+++
date = "2016-11-13T17:12:38+01:00"
description = "kill-switch on asus router"
title = "asuswrt merlin"
tags = ["asus", "merlin", "routers", "openvpn"]
draft = true

+++


# Kill-switch on ASUS routers asuswrt-merlin

[asuswrt-merlin](https://github.com/RMerl/asuswrt-merlin) is an enhanced version
of Asuswrt - the firmware used by all recent Asus routers.

At this time, the supported devices are:

- RT-N66U
- RT-AC66U
- RT-AC56U
- RT-AC68U & RT-AC68P (including revision C1)
- RT-AC87U
- RT-AC3200
- RT-AC88U
- RT-AC3100
- RT-AC5300
- RT-AC1900 & RT-AC1900P


Firmware builds can be downloaded from the official download site:

https://asuswrt.lostrealm.ca/download

# Setup

On the VPN/OpenVPN settings, after configuring a client, set the option
``Firewall`` to ``custom``:

![custom firewall](/img/asus-openvpn-firewall.jpg)

On the ``Redirect Internet traffic`` select the option ``Policy Rules`` and add
bellow the devices you would like to route all their traffic thought the VPN:

![policy rules](/img/asus-openvpn-policy-rules.png)


Enable the option ``Enable JFFS custom scripts and configs`` located in
[Administration/System](http://192.168.1.1/Advanced_System_Content.asp)

![jffs custom scripts](/img/asus-jffs-custom-scripts.png)

# firewall-start

Via SSH login to your router and edit/create the file ``/jffs/scripts/firewall-start``

```sh
#!/bin/sh

sleep 4

iptables -I FORWARD -i br0 -o tun11 -j ACCEPT
iptables -I FORWARD -i tun11 -o br0 -j ACCEPT
iptables -I FORWARD ! -o tun11 -s 192.168.xx.xxx -j DROP
iptables -I FORWARD ! -o tun11 -s 192.168.xx.xxx -j DROP
iptables -I INPUT -i tun11 -j REJECT
iptables -t nat -A POSTROUTING -o tun11 -j MASQUERADE
```

Replace the ``192.168.xx.xxx`` with your IP's.


To test, disconnect the VPN and verify that the selected devices to use the VPN,
are not available to connect, re-connect the VPN and check the IP using any of this sites:

- https://www.dnsleaktest.com/
- http://geoip.hidemyass.com/
- https://tuip.info
- http://checkip.amazonaws.com/

---

{{< youtube oBLS7Wkn0C4>}}

---

More info: <https://github.com/RMerl/asuswrt-merlin/wiki/Policy-based-routing-(manual-method)>
