---
layout: default
title: Nmap
parent: Linux
has_children: false
---

# Nmap

## find hosts with SSHd enabled

```bash
nmap -p 22 192.168.1.0/24 -oG - | grep 22/open
```

## ping a range

```bash
nmap -sP -R 192.168.1.1-254
```
