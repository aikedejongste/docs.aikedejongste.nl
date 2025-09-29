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

## Compare PSU efficiency

[Wolfgang's List](https://docs.google.com/spreadsheets/d/1TnPx1h-nUKgq3MFzwl-OOIsuX_JSIurIq3JkFZVMUas/edit#gid=110239702)


## ECC Memory

If you're on Linux, dmesg containing this is a pretty reliable indication that ECC is working.

`EDAC MC0: Giving out device to module amd64_edac`
