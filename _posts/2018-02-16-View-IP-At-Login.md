---
title: View Your IP (Or Anything Else) At Login
date: 2018-02-16 00:00:00 Z
layout: post
---

When I started setting up a new network with ESXi, I wanted to make everything as uniform as possible. When I started up a new VM, I wanted to be able to SSH in as soon as it was ready. This plan was foiled by needing to log in through the console and get the IP or look at my current DHCP leases. Place these lines in `/etc/rc.local` to show the IP address at the login prompt.

```bash
echo "Current IP is:" > /etc/issue
ifconfig ens160 | awk '/inet addr/ {print $2}' | cut -f2 -d: >> /etc/issue
echo "" >> /etc/issue
```

Make sure to change `ens160` to the correct interface.
