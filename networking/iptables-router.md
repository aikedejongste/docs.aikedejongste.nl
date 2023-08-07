---
layout: default
title: IPtables router
parent: Networking
---

# IPtables routing

## IPtables NAT

```bash
#!/bin/bash
set -x

# Public Interface
EXT=eth0
# Private Interface
INT=ens10

# DST is usually internal IP in local lan
DST=10.10.1.2
# SRC is usually public IP on the internet
SRC=60.70.80.90

# Source Port on Public interface
SPT=2222
# Destination Port on internal network
DPT=32222

echo "1" > /proc/sys/net/ipv4/ip_forward

# open a port, not required
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -i $EXT -o $INT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A POSTROUTING -t nat -o $EXT -j MASQUERADE

# forward port to internal host
iptables -t nat -A PREROUTING -i $EXT -p tcp --dport $SPT -j DNAT --to-destination $DST:$DPT
iptables -A FORWARD -i $EXT -o $INT -p tcp --dport $SPT -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

```

## Or like this

```bash
sudo iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth1 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o eth1 -j ACCEPT
```

These rules do the following:

The first rule enables NAT (network address translation) for traffic from the internal network (10.0.0.0/24) that is going out through the eth0 interface, which is connected to the internet. The MASQUERADE target causes the source IP address of the outgoing packets to be translated to the public IP address of the eth0 interface.

The second rule allows traffic that is related or established (i.e., responses to outgoing traffic) to pass through the firewall in both directions between the internal network (eth1) and the internet (eth0).

The third rule allows all other traffic to pass through the firewall in both directions between the internal network (eth0) and the internet (eth1).
