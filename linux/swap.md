---
layout: default
title: Swap
has_children: false
parent: Linux
---

# Swap

## Create and apply a 5GB swapfile
```dd if=/dev/zero of=/swapfile1 bs=1024 count=5242880 && chown root:root /swapfile1 && chmod 0600 /swapfile1 && mkswap /swapfile1 && swapon /swapfile1```

## Add Swapfile to fstab
```echo "/swapfile1 swap swap defaults 0 0" >> /etc/fstab```
