+++
title = "About"
description = "Why?"
date = "2016-11-12T15:19:37+01:00"

+++

# What is a VPN kill switch?

> A kill switch, also known as an emergency stop or e-stop, is a safety mechanism used to shut off a device or machinery in an emergency situation in which it cannot be shut down in the usual manner. https://en.wikipedia.org/wiki/Kill_switch

A **VPN Kill-Switch** is a critical piece of your VPN security solution. Also
known as an Internet Kill Switch, this will make sure that your true
[IP address](http://checkip.amazonaws.com/) is never exposed online in the
event of a dropped VPN connection.

Some VPN providers offer this as feature that in theory automatically "kills"
your internet connection if the VPN fails or get disconnected, but if the
application crashes or quit unexpectedly, high are the changes that the
**kill-switch** will never be used and therefore your traffic / IP address become
exposed.

![ipvanish](/img/ipvanish-vpn-quit-unexpectedly.png)


There are many VPN providers the difference is mainly the price, but when it
comes to security it is better to use a **kill switch** at an operating system
level rather than depending on an APP that eventually may fail or crash leaving
you vulnerable.

The goal of this site is to share best practices about how to implement,
create a **kill switch** not depending on any VPN provider/application so it
doesn't mater if the VPN drops crashes you will not be vulnerable.
