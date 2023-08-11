---
layout: default
title: Proxmox
parent: Virtualization
has_children: false
---

# Proxmox

## Resolve locked vm problem

``bash

qm unlock 105

rm /var/lock/qemu-server/lock-105.conf

qm stop 105

```
