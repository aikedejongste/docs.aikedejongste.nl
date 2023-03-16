---
layout: default
title: Linux
has_children: true
---

# Linux


## Download and verify iso

```bash
wget http://cdimage.ubuntu.com/ubuntu/releases/18.04/release/SHA256SUMS.gpg
wget http://cdimage.ubuntu.com/ubuntu/releases/18.04/release/SHA256SUMS

gpg2 --recv-keys 0x46181433FBB75451 0xD94AA3F0EFE21092
gpg2 --verify SHA256SUMS.gpg SHA256SUMS

sha256sum -c SHA256SUMS 2>&1 | grep OK
```

