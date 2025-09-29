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

## Mount partition in image

```bash
losetup -fP full_disk.img
cryptsetup luksOpen /dev/loop0p1 naamvanjekeuze
mkdir /mnt/naamvanjekeuze
mount /dev/mapper/naamvanjekeuze /mnt/naamvanjekeuze
```

And when there is LVM in the image:

```bash
vgscan
vgchange -ay
mount /dev/mapper/vgname-lvname /mnt/naamvanjekeuze
```

## Umount

```bash
umount /mnt/naamvanjekeuze
cryptsetup luksClose naamvanjekeuze
losetup -d /dev/loop0
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
