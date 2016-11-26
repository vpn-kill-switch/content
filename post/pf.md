+++
date = "2016-11-12T16:24:58+01:00"
title = "PF macOS & BSD"
description = "kill switch using PF"
tags = ["BSD", "apple", "macOS", "pf"]

+++

For [Mac OS X](https://apple.com/mac/) &
[BSD](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution)
users [PF](https://www.freebsd.org/doc/handbook/firewalls-pf.html)
can be the best option to create a **kill switch**.

> PF (Packet Filter, also written pf) is a BSD licensed stateful packet filter,
a central piece of software for firewalling. It is comparable to netfilter
(iptables), ipfw and ipfilter. PF was developed for OpenBSD, but has been ported
to many other operating systems. https://en.wikipedia.org/wiki/PF_(firewall)

To create a **kill switch** using PF, a [configuration file](https://www.openbsd.org/faq/pf/)

> TL;DR
[killswitch](/post/killswitch/) is a CLI command that can help finding the
active interfaces, create and load pf rules.

Example of a ``killswitch.pf.conf`` configuration file:

> The file can be creted on the user $HOME directory and have any name.

```sh
int_en1 = "en1"
vpn_utun1 = "utun1"
vpn_ip = "1.2.3.4"

set block-policy drop
set ruleset-optimization basic
set skip on lo0

block all

# dns
pass quick proto {tcp, udp} from any to any port 53 keep state

# Allow broadcasts on internal interface
pass from any to 255.255.255.255 keep state
pass from 255.255.255.255 to any keep state

# Allow multicast
pass proto udp from any to 224.0.0.0/4 keep state
pass proto udp from 224.0.0.0/4 to any keep state

# Allow ping
pass on $int_en1 inet proto icmp all icmp-type 8 code 0 keep state

# use only the vpn
pass on $int_en1 proto {tcp, udp} from any to $vpn_ip
pass on $vpn_utun1 all
```

The first 3 lines need to be changed/adjusted based on the running system.

```sh
int_en1 = "en1"     # active interface
vpn_utun1 = "utun1" # VPN interface
vpn_ip = "1.2.3.4"  # VPN peer IP
```

After editing the ``killswitch.pf.conf`` file, the PF rules can be loaded by
running:

	$ sudo pfctl -Fa -f ~/killswitch.pf.conf -e

To disable:

	$ sudo pfctl -d

To load your system defaults (no kill switch)

	$ sudo pfctl -Fa -f /etc/pf.conf -e

In some cases where the VPN peer IP is random, the VPN needs to be connected
first and later load the **kill switch** PF rules, otherwise wont be possible to
connect.

# active interface

In this example ``int_en1`` is the active interface (WiFi) to find the
active interfaces run the following command within a terminal:

```sh
$ route get 0.0.0.0 2>/dev/null | awk '/interface: / {print $2}';
```

or

```sh
$ ifconfig | pcregrep -M -o '^[^\t:]+:([^\n]|\n\t)*status: active'
```

This commands can give more information:

```sh
$ networksetup -listnetworkserviceorder
```

```sh
$ scutil --dns
```

# VPN interface

After connecting to the VPN, by running the command ``ifconfig -a`` and
searching for ``POINTOPOINT``, could help to find the interface used by the VPN:

```sh
$ ifconfig -a | grep -i POINTOPOINT
gif0: flags=8010<POINTOPOINT,MULTICAST> mtu 1280
utun0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 2000
utun1: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1500
```

In this example there are 3 interfaces, but in many cases only 1 will be active
and with an assigned [IPv4](https://en.wikipedia.org/wiki/IPv4), by using the
command ``ifconfig <interface_name>>`` this can be found, example:

```sh
$ ifconfig utun1
utun1: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1500
	inet 172.21.21.233 --> 172.21.21.233 netmask 0xfffffe00
```

# VPN PEER IP

This is the IP of the VPN server providing the service, after connecting to the
VPN many apps display this IP:

![peerIP](/img/peerIP.jpg)

Once knowing the active interface this can be double checked by using the
command ``netstat``, example:

```sh
$ netstat -rna -f inet | grep en1
default            192.168.1.1        UGSc            7        2     en1
143.255.56.99/32   192.168.1.1        UGSc            2        0     en1
143.255.56.99      192.168.1.1        UGHWIi          2     5239     en1
```
