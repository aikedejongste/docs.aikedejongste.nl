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
apt-get install cifs-utils samba-common

or

apt install cifs-utils psmisc
```

## Linux Server

### Install dependencies

```bash
apt install samba
```

### UTF-8

It is recommended to mount shares with `iocharset=utf8`.

For that you probably need to `apt install linux-modules-extra-$(unane -r)`.

Don't forget to reboot before installing the linux-modules-extra or type the correct uname manually.

### Mount temporariy

```bash
mount -t cifs //192.0.2.17/SharedFiles /mnt/smb_share
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

Systemd service called `samba` and `smb` are not the same.

- samba = Active Directory service
- smb = file sharing

### Don't set password for smb user

These are useless if you don't disable guest logins. And you're probably
better of setting passwords for the system users. They will sync automatically
on Debian/Ubuntu.

```bash
smbpasswd -a aike
```
