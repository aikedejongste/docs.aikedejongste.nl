---
layout: default
title: APT
has_children: false
parent: Linux
---

# APT

## Fun fact

.deb packages are just ar files with a funky header (d.e.b) and some pre/post scripts. Ar files are also .o files after compilation but before linking, what!?! Also, tar is based on Ar, WHAT!?! Elf files, the whole thing is standardized!!

## Automatic updates and upgrades

Full explanation here: [Debian Wiki](https://wiki.debian.org/UnattendedUpgrades)

Run as root:

```bash
apt-get install unattended-upgrades apt-listchanges
echo unattended-upgrades unattended-upgrades/enable_auto_updates boolean true | debconf-set-selections
dpkg-reconfigure -f noninteractive unattended-upgrades
```

## Wait for apt-get to be available

```bash while ! apt-get -qq check; do sleep 1s; done```

## Enable automatic upgrades with cli

```bash apt install unattended-upgrades && dpkg-reconfigure -plow unattended-upgrades```

## Enable automatic upgrades with config

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

## Sort installed Debian packages by size

```bash
dpkg-query -W --showformat='${Installed-Size} ${Package}\n' | sort -n
```
