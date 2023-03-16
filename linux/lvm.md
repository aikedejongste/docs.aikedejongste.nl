---
layout: default
title: LVM
has_children: false
parent: Linux
---

# LVM

## LVM Snapshot script

I think this was written by Joris, thanks!

It creates a snapshot and uploads it to a SFTP location.

```bash
#!/bin/bash

set -e

USER=u239875
MOUNTPOINT=/mnt/offsite
VG=vg-ssd-500GB
LV_BACKUP=vm-backup

if [ -z "$1" ]; then
  echo "Usage: $0 volume-name"
  exit 1
fi

LV="$1"

if [ -e "/dev/$VG/$LV_BACKUP" ]; then
  echo "/dev/$VG/$LV_BACKUP still exists! Is another backup running?"
  echo "If not, please run:"
  echo "lvremove /dev/$VG/$LV_BACKUP"
  exit 1
fi

if [ ! -d "$MOUNTPOINT/.ssh" ]; then
  sshfs $USER@$USER.your-storagebox.de:/ $MOUNTPOINT
fi

set -x

/sbin/lvcreate --size 50G --snapshot --name $LV_BACKUP "/dev/$VG/$LV"

pv "/dev/$VG/$LV_BACKUP" | nice -n 19 pigz -p 2 > "$MOUNTPOINT/$LV.gz.new"
mv "$MOUNTPOINT/$LV.gz.new" "$MOUNTPOINT/$LV.gz"

/sbin/lvremove -y "/dev/$VG/$LV_BACKUP"

ls -lh $MOUNTPOINT
df -h $MOUNTPOINT

umount $MOUNTPOINT
```
