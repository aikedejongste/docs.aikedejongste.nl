---
layout: default
title: LUKS cryptsetup
has_children: false
parent: Linux
---

# Mount an encrypted drive

## Install

```bash
sudo apt install cryptsetup
```

## Decrypt

```bash
cryptsetup luksOpen /dev/sdc1 sandisk-1tb
```

## Mount

```bash
mount /dev/mapper/sandisk-1tb /mnt/sandisk-1tb
```

## Encrypt

```bash
cryptsetup luksFormat /dev/sdc1
```

## With Ansible

```yaml
---
- name: Mount LUKS encrypted drive
  hosts: your_target_host
  tasks:
    - name: Install required packages
      apt:
        name: cryptsetup
        state: present

    - name: Open LUKS container
      command:
        cmd: "cryptsetup luksOpen /dev/sdXN your-mapped-name"
        creates: "/dev/mapper/your-mapped-name"

    - name: Mount the drive
      mount:
        path: /mount/point
        src: /dev/mapper/your-mapped-name
        fstype: ext4
        state: mounted
```
