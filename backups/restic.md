---
layout: default
title: Restic
parent: Backups
---

# Restic

Useful wrapper for healthchecks: [restic-tools](https://github.com/binarybucks/restic-tools)

Run with:

```bash backup hetzner local```

## SSH config for Hetzner

in `~/.ssh/config`

```
Host backup-storagebox
  User u12356-sub1
  IdentityFile /home/aike/.ssh/restic.key
  HostName u12345.your-storagebox.de
  Port 23
```
