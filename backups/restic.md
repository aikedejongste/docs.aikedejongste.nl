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

## Backblaze keys without deleteFiles:

`b2 create-key --bucket <bucketName> <keyName> listBuckets,readFiles,writeFiles,listFiles`


## First run
First run `rclone serve restic --append-only` to receive backups via http from the production cluster.

## Second run
Second, run `restic forget --prune` using read/write file access methods, such as SFTP or locally mounted files.

`rclone serve restic --append-only`

`restic forget --keep-last 1 --prune`
restic stats --mode raw-data
restic snapshots --path="/home"

restic forget --keep-daily 7 --keep-weekly 5 --keep-monthly 12 --keep-yearly 2

`apt install restic && restic self-update`



## GUI's
* [Restic Backup GX](https://gitlab.com/stormking/resticguigx/-/blob/master/README.md)
	* nice for Windows users, use "Repository Input" to use remote targets
* [NPBackup](https://github.com/netinvent/npbackup)
* [Restic Browser](https://github.com/emuell/restic-browser)

