---
layout: default
title: Cloud-Init
parent: Cloud Infrastructure
---

# cloud-init

## Example cloud-init config

```bash
package_update: true
package_upgrade: true

packages:
- htop
- vim
- curl
- rsync
- vnstat
- git
- ufw

users:
  - name: aike
    gecos: Aike de Jongste
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: users, admin
    ssh_import_id: None
    shell: /bin/bash
    lock_passwd: true
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMJZhBxjBZgaU5JQWaS2smXC9IFS46jR5jVdDYHyq8DS

bootcmd:
  - swapoff -a
  - cp /etc/fstab /etc/fstab.bak
  - sed -i '/SWAP/d' /etc/fstab
  - rm -f /etc/machine-id /var/lib/dbus/machine-id && dbus-uuidgen --ensure=/etc/machine-id && dbus-uuidgen --ensure
  - git config --global user.email "aikedejongste@gmail.com"
  - git config --global user.name "Aike de Jongste"

```
