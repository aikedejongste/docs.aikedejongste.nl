---
layout: default
title: Smartctl
has_children: false
parent: Linux
---

# Smartmontools (smartctl)

## Update database

```bash
sudo update-smart-drivedb
```

## Enable service

```bash
systemctl umask smartmontools && systemctl start smartmontools
```
