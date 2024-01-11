---
layout: default
title: Restic
parent: Backups
---

# Restic

## Variables

```bash
#!/bin/bash

export RESTIC_REPOSITORY="b2:repo-name"
export RESTIC_PASSWORD="aaaaa"
export B2_ACCOUNT_ID="bbbbb"
export B2_ACCOUNT_KEY="ccccc"

restic .....
```

## Show hosts in repository

```bash
restic ...
```

## Remove all snapshots for a host from repository

Run without prune first! You cannot delete all snapshots at once. You have to specify a keep policy.

```bash
restic forget --host 'your_host_name' --keep-last 1 --path /opt
restic forget <id>
restic prune
restic check
```

## Init new repository

```bash
restic init
```

## Mount repository

```bash
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
