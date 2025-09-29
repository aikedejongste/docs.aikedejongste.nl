---
layout: default
title: Tilaa
parent: Cloud Infrastructure
---

# Tilaa

## Floating IPs

On their Debian install.

```
source /etc/network/interfaces.d/*

# loopback
auto lo
iface lo inet loopback

# primary NIC
auto ens3
iface ens3 inet static
  address 1.2.3.4
  netmask 255.255.255.0
  gateway 84.22.102.1
  dns-nameservers 84.22.106.7 84.22.106.8
  post-up ip addr add 4.5.6.7/24 dev ens3
  post-up ip addr add 8.9.0.1/24 dev ens3

iface ens3 inet6 static
  address 2a02:2770:1xxxxxxx
  netmask 64
  gateway 2a02:2770:17::1
  post-up ip -6 addr add 2a02:2770:xxxx:0:1/64 dev ens3
  post-up ip -6 addr add 2a02:2770:xxxx:0:2/64 dev ens3
```
