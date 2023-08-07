---
layout: default
title: Netplan WAN+LAN
parent: Networking
---

# Netplan with public and private IP addresses

This works on:

* Digital Ocean

```yaml
network:
    version: 2
    ethernets:
        eth0:
            addresses:
            - 159.89.119.58/20
            - 10.20.0.6/16
            match:
                macaddress: f2:81:84:26:d9:44
            mtu: 1500
            nameservers:
                addresses:
                - 67.207.67.3
                - 67.207.67.2
                search: []
            routes:
            -   to: 0.0.0.0/0
                via: 159.89.112.1
            set-name: eth0
        eth1:
            addresses:
            - 10.118.0.3/20
            match:
                macaddress: 0a:1e:33:6b:8e:af
            mtu: 1500
            nameservers:
                addresses:
                - 67.207.67.3
                - 67.207.67.2
                search: []
            set-name: eth1
```
