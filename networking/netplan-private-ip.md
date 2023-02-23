---
layout: default
title: Netplan private ip
parent: Networking
---

# Netplan private ip only configs

## General suggestions

In some versions use this for IP address:
```
      addresses: [192.168.11.13/24]
```

In some versions you have to use default for the route:
```
      routes:
        - to: default
          via: 192.168.11.1
```

## Netplan Hetzner private only config

This doesn't work for some reason. Suggestions welcome.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens10:
      optional: false
      addresses: 
      - 10.10.1.6/32
      routes:
      - to: 0.0.0.0/0
        via: 10.10.1.1
        on-link: true
      nameservers:
        addresses: [8.8.8.8]
      dhcp4: no
      dhcp6: no
```

