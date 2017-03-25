+++
date = "2017-03-25T13:18:30+01:00"
description = "sudo killswitch -e"
title = "How it works"

+++

**killswitch**, in a nutshell, once enabled, it will allow only traffic within the
VPN, if the [VPN goes down](/about) or it crashes, your traffic / IP address will not
become exposed.

Before enabling the kill switch `killswitch -e` the VPN needs to be up and running.

By just running `killswitch`, information about the existing interfaces and
public IP address is printed, example:

```html
$ killswitch
Interface  MAC address         IP
en1        ac:21:37:b1:81:b4   192.168.1.50

Public IP address: 91.171.85.85

No VPN interface found, verify VPN is connecte
```

# DNS leak

Every time `killswitch` is executed, it will try to obtain the public IP address
via DNS or HTTP, if they differ both IP's will be printed, example:

```html
$ killswitch
Interface  MAC address         IP
en1        ac:21:37:b1:81:b4   192.168.1.50

DNS leaking:
Public IP address (DNS): 5.255.241.137
Public IP address (WWW): 91.171.85.85

No VPN interface found, verify VPN is connected
```

See more: https://en.wikipedia.org/wiki/DNS_leak

# Enable

Once VPN is un and running, run:

    $ sudo killswitch -e


# Disable

If VPN crashes or disconnects you may not be available to reconnect again unless you disable the kill switch.

**WARNING** Before disabling the kill switch, ensure you don't have any application "P2P" running that may compromise your traffic/IP.

    $ sudo killswitch -d

This will load the default firewall rules defined on `/etc/pf.conf`. After doing this you may try to connect again the VPN and re-enable the kill switch.

Custom rules may be loaded by using something like:

```sh
$ sudo pfctl -Fa -f /etc/custom-pf.conf
```
