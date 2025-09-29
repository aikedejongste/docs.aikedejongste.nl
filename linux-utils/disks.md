---
layout: default
title: Storage
has_children: false
parent: Linux
---

# Storage

## dd disk to image

TODO: test sparse!

```bash
dd if=/dev/nvme0n1 of=/media/user/storage-disk/full-disk.img conv=sync bs=512K status=progress
```

## Resize partition

```bash
apt install gdisk cloud-guest-utils
growpart --dry-run /dev/sda 1
growpart /dev/sda 1
e2fsck -f /dev/sda1
resize2fs /dev/sda1
```

## List disk makes and models

```bash
for dev in /sys/class/scsi_device/*/device/model; do
  echo -n "Model for device ${dev##*/}: "
  cat "$dev"
done
```

## Format as exFAT

After installing this the Gnome Disk utility also supports exFAT if you choose 'other'.

```bash
sudo apt install exfat-fuse exfatprogs
sudo mkfs.exfat /dev/sdX
```

## Alert script for disk usage

```bash
#!/bin/bash

USEDSPACE=$(df -h | awk '{print $5}' | grep -vi use | cut -d'%' -f1)
for i in $USEDSPACE
do
     if [ $i -gt 90 ]
     then
          (df -h | head -n1; df -h | grep $i; \
          echo; echo "Total Disk Space Used By /tmp = $(du -sh /tmp | awk '{print$1}')"; \
          echo; echo "Total Disk Space Used By /var/log = $(du -sh /var/log | awk '{print$1}')") \
          | mail -s "HDD Space Warning For $(hostname -f)” alert@company.nl

     elif [ $i -gt 85 ]
     then
          (df -h | head -n1; df -h | grep $i) \
          | mail -s "HDD Space Warning For $(hostname -f)" alert@company.nl

     elif [ $i -gt 80 ]
     then
          (df -h | head -n1; df -h | grep $i) \
          | mail -s "HDD Space Warning For $(hostname -f)" alert@company.nl

     fi
done
```
