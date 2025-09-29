---
layout: default
title: Disk Images
has_children: false
parent: Linux
---

# Disk Images

## Mount partition in a disk image

If your disk image has multiple partitions (for example, an .img file), you can set up a loop device with partition support, then mount the specific partition you want.

```bash
sudo losetup -fP diskimage.img
```

The -f picks a free loop device.
The -P scans the partition table and creates devices like /dev/loop0p1, /dev/loop0p2, etc.

```bash
ls /dev/loop0*
```

You should see something like /dev/loop0p1, /dev/loop0p2, etc.

```bash
sudo mount /dev/loop0p1 /mnt/partition1
```

And unmount when you’re done:

```bash
sudo umount /mnt/partition1
sudo losetup -d /dev/loop0
```
