---
layout: default
title: DHCP
parent: Networking
---

# DHCP

## See leases

```bash
cat /var/lib/dhcp/dhclient.leases
```

## Release lease

```bash
sudo dhclient -r -v
```

## Renew lease

```bash
sudo dhclient -v
```

or

```bash
sudo dhclient wlp0s20f3 -v
```
