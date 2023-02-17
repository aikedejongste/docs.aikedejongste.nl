---
layout: default
title: Debian private ip only
parent: Networking
---

# Debian private ip only

This works on:
* Tilaa

```
# The loopback network interface
auto lo
iface lo inet loopback
 
# The primary network interface
auto enp0s5
iface enp0s5  inet static
  address 192.168.2.236
  netmask 255.255.255.0
  gateway 192.168.2.254
  dns-domain aike.lan
  dns-nameservers 1.1.1.1
```

```bash
systemctl restart networking
```

