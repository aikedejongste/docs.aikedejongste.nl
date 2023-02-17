---
layout: default
title: APT
#nav_order: 3
has_children: false
---

# APT

# Wait for apt-get to be available.

while ! apt-get -qq check; do sleep 1s; done

## Enable automatic upgrades with cli:

```apt install unattended-upgrades && dpkg-reconfigure -plow unattended-upgrades```

## Enable automatic upgrades with config:

- `/etc/apt/apt.conf.d/20auto-upgrades` should contain 2 lines

```bash
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

- configure with `/etc/apt/apt.conf.d/50unattended-upgrades`
... something


## Purge uninstalled packages on Ubuntu

```bash
dpkg --list |grep "^rc" | cut -d " " -f 3 | xargs sudo dpkg --purge
```

## Sort installed Debian packages by size:

```bash
dpkg-query -W --showformat='${Installed-Size} ${Package}\n' | sort -n
```

