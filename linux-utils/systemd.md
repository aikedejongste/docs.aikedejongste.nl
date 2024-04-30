---
layout: default
title: Systemd
has_children: false
parent: Linux
---

# Systemd

## Cleanup Systemd journal

```bash
journalctl --vacuum-time=1d
```

## Failed units

```bash
systemctl list-units --state=failed
```

``bash
systemctl reset-failed
```

## List services

```bash
systemctl list-unit-files --type service --all
```
