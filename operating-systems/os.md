---
layout: default
title: Operating Systems
has_children: true
---

# Operating Systems

## Linux Distributions

### Flatcar Linux

### Debian Live USB

Download ISO here [torrent](https://cdimage.debian.org/debian-cd/current-live/amd64/bt-hybrid/) or here [direct](https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/). It is about 3GB.

```bash
cp debian-live.iso /dev/sda
```

Alternatively you can also use dd:

```bash
sudo dd if=<file> of=<device> bs=16M status=progress oflag=sync
```

### Debian Install USB


