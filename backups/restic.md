---
layout: default
title: Restic
parent: Backups
---

# Restic

## Mount repository

For example when it is on Backblaze B2.

```bash
#!/bin/bash

export RESTIC_REPOSITORY="b2:repo-name"
export RESTIC_PASSWORD="aaaaa"
export B2_ACCOUNT_ID="bbbbb"
export B2_ACCOUNT_KEY="ccccc"

restic mount /mnt/restic
```

## Wrapper

Useful wrapper for healthchecks: [restic-tools](https://github.com/binarybucks/restic-tools)

Run with:

```bash
backup hetzner local
```

## SSH config for backups to Hetzner storage box

in `~/.ssh/config`

```
Host backup-storagebox
  User u12356-sub1
  IdentityFile /home/aike/.ssh/restic.key
  HostName u12345.your-storagebox.de
  Port 23
```

