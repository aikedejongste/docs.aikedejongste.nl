---
layout: default
title: Hetzner Cloud Init Docker Host
parent: Cloud Infrastructure
has_children: false
---

# Hetzner with Cloud Init

## Cloud Init file

```yaml
#cloud-config
groups:
  - docker
users:
  - name: aikedejongste
    groups: sudo,docker
    sudo: ALL=(ALL) NOPASSWD:ALL
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMJZhBxjBZgaU5JQWaS2smXC9IFS46jR5jVdDYHyq8DS
package_update: true
package_upgrade: true
packages:
  - vim
  - git
  - vnstat
  - docker-ce
  - nload
  - htop
  - screen
  - apparmor-utils

  run: add docker repo
```
