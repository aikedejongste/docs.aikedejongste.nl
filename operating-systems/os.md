---
layout: default
title: Operating Systems
has_children: true
---

# Operating Systems

## Easy way to install OS from USB

You install Ventoy to the usb disk once and later add ISO files. And it
will create a boot menu for you.

- [Ventoy](https://www.ventoy.net/en/index.html)

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

Use Ventoy!

### CentOS / RHEL / AlmaLinux

Undo package update:

```bash
yum history list
yum history undo <id>
```
