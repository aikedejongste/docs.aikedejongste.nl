---
layout: default
title: IPtables router
parent: Networking
---

# IPtables routing

## IPtables NAT:

```bash
#!/bin/bash
INT=eth1
EXT=eth0

echo "1" > /proc/sys/net/ipv4/ip_forward

iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -i $EXT -o $INT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A POSTROUTING -t nat -o $EXT -j MASQUERADE
```
