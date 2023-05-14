---
layout: default
title: Cloud-Init
parent: Infrastructure
---

# cloud-init


## Example cloud-init config

```
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
      - ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAsFMSthnrlRdXwvQMgDNZXXn9e3sfivmN7YcDtZAGnNHsfwxmfCzTfEO95Ee9dZTkyRODLYl/ivIUBLpkmi8QP9B8haSedZuNHc4yXM75iaCkel5yC1Xc4+Czq9yLTdFyJxB/kAKpjfkZKh5LmOm+7NbK1AjRM2KdobkhWG+BTCPP3c8EX7ztOLLzs23iIL85w5eKvXFjYftfnaDskUFiORU+W58KlcA+2TVc+v0MmdZ1pLmNZuaXFrHcSBj3/mk30vuvjsH5KTy5bGCwHIGID2xB2b5JaE2Wc7dEF/L4Z0Ybkty3mlkx0/raixrS1nGZN10kxZu72I+J+RgZhChAow== 

bootcmd:
  - swapoff -a
  - cp /etc/fstab /etc/fstab.bak
  - sed -i '/SWAP/d' /etc/fstab
  - rm -f /etc/machine-id /var/lib/dbus/machine-id && dbus-uuidgen --ensure=/etc/machine-id && dbus-uuidgen --ensure
  - git config --global user.email "aikedejongste@gmail.com"
  - git config --global user.name "Aike de Jongste"

```

