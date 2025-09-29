---
layout: default
title: Rsync
has_children: false
parent: Linux
---

# Rsync

## Notes

- the `/` at the end of the source dir signal that you want to copy the contents, not the dir itself.

## Options

- -P = progress and patial
- -a = archive

## Local sync

```bash
rsync -aP /opt/source/ /opt/destination
```

## From remote to local

```bash
rsync -vuar --progress user@remote:/source /opt/destination
```

## From local to remote

...

## In a script with locking

```bash
#!/bin/bash

LOCKFILE="/var/run/something.lock"

SOURCE_DIR="/opt/source"
DEST="-e \"ssh -q -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null\" username@hostname:/opt/dest"
RSYNC_CMD="rsync -v -a --delete -O --chmod=u=rw,go=r,D+x $SOURCE_DIR/ $DEST"
flock -x -n $LOCKFILE -c "${RSYNC_CMD}"
```
