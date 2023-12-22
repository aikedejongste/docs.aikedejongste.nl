---
layout: default
title: Hardware
has_children: true
---

# Hardware

## Check USB port speed in Linux

```bash
lsusb -vvv |grep -i -B5 -A5 bcdUSB
```
