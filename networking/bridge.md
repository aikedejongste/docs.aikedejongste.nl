---
layout: default
title: Bridge
parent: Networking
---

# Hetzner network bridge

cat /etc/network/interfaces

```bash
auto lo
iface lo inet loopback

auto enp0s31f6
iface enp0s31f6 inet static
  address 1.2.3.4
  netmask 255.255.255.192
  gateway 5.6.7.8
  up route add -net 9.1.2.3 netmask 255.255.255.192 gw 5.6.7.8 dev enp0s31f6
  up ethtool -K enp0s31f6 tso off gso off

auto hetzner0
iface hetzner0 inet manual
  bridge_ports none
  bridge_stp off
  bridge_waitport 0
  bridge_fd 0
  post-up ip route add 11.12.3.64/32 dev $IFACE
  pre-down ip route del 11.12.3.4.64/32
```
