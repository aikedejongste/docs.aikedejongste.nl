---
layout: default
title: fstab
has_children: false
parent: Linux
---

# fstab

## Find disk UUID

```bash
blkid | grep UUID
```

## Numbers at the end of the lines

1. The fifth field, (fs_freq), is used for these filesystems by the dump(8) command to determine which filesystems need to be dumped. So basically not used anymore.
2. The sixth field, (fs_passno), is used by the fsck(8) program to determine the order in which filesystem checks are done at reboot time. The root filesystem should be specified with a fs_passno of 1, and other filesystems should have a fs_passno of 2. Filesystems within a drive will be checked sequentially, but filesystems on different drives will be checked at the same time to utilize parallelism available in the hardware. If the sixth field is not present or zero, a value of zero is returned and fsck will assume that the filesystem does not need to be checked.


