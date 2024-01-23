---
layout: default
title: Networking
has_children: true
---

# Networking

## Create a 1000MB.bin for speed testing

```bash
dd if=/dev/urandom of=1000MB.bin bs=64M count=16 iflag=fullblock
```

## Disable cloud-init after changing network config

```bash
touch /etc/cloud/cloud-init.disabled
```

## Open Ngrok tunnel

```bash
ngrok http --region=eu --hostname=aike.eu.ngrok.io 4567
```

## Show usage on Ubuntu

* nload
* speedometer
* iftop
* nethogs
* bmon
* slurm
* tcptrack
* iptraf
