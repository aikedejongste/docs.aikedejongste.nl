---
layout: default
title: Wipe / shred
has_children: false
parent: Linux
---

# Securely wiping systems

## With shred

The 0 fills the disk with zeroes at the end to hide shredding. Seems to work
fine over SSH on a running system!

```bash
shred -vzn 0 /dev/sda
```
