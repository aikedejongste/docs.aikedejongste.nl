---
layout: default
title: DNS
parent: Networking
---

# DNS

## Check if the txt record has been processed

```bash
host -t txt _acme-challenge.example.com
```

## Show SOA records

```bash
host -t soa domain.com
```

## DNS on Debian Bookworm

Ubuntu differs from Debian!

```bash
apt install systemd-resolved
```

## DNS on Ubuntu using netplan

Maybe the better approach?


## DNS on Ubuntu using resolvconf

Don't use resolvconf on Debian.

Systemd priorizes on-link DNS server over global DNS server over global fallback DNS servers in its default settings.

```bash
apt install resolvconf
```

Make sure that /etc/resolv.conf is a symlink to /run/resolvconf/resolv.conf

```bash
ln -s /run/resolvconf/resolv.conf /etc/resolv.conf
```
