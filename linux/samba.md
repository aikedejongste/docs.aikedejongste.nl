---
layout: default
title: Samba / CIFS
has_children: false
parent: Linux
---

# Samba / CIFS

## Linux Client

### Install dependencies

```bash
sudo apt-get install cifs-utils samba-common
```

### Mount with credentials

Put them somewhere safe, for example  `/root/.smb`.

```
user=linux-client
password=12345678
domain=company.com
```

### FSTAB

Use `\040` for a space.

```
//server/share         /mnt/here           cifs    uid=999,gid=1001,credentials=/root/.smb,iocharset=utf8,vers=2.1         0 0
```

## Server
