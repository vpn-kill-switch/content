+++
title = "killswitch"
description = "brew install killswitch"
date = "2016-11-12T21:15:48+01:00"
tags = ["cli", "tools", "brew"]

+++


**killswitch** a CLI tool for creating & loading pf rules.

To install on Mac OS X:

    $ brew tap vpn-kill-switch/killswitch

Next:

    $ brew install killswitch

Download from https://dl.bintray.com/nbari/killswitch/ or compile from source: https://github.com/vpn-kill-switch/killswitch


**killswitch** helps to find the active interfaces and creates a
``.killswitch.pf.conf`` file.

The current version is very simple it requires to specify the VPN endpoint IP
``-ip`` and it can also load the generated rules by specifying option ``-e``

```sh
$ killswitch -h
Usage of killswitch:
  -e Enable
        Enable load the pf rules
  -ip IPv4
        VPN peer IPv4
  -v    Print version: 0.1.0
```

If using option ``-e`` [sudo](https://en.wikipedia.org/wiki/Sudo) may be required:

	$ sudo killswitch -ip 1.2.3.4 -e
