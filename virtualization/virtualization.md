---
layout: default
title: VirtualBox
has_children: false
parent: Virtualization
---

# VirtualBox

## Convert vhdx to vhd

`VBoxManage.exe clonemedium "input_file.vhdx" "output_file.vhd" --format VHD`

## Convert vhdx to raw with Qemu

`apt install qemu-utils`

`qemu-img convert -O raw OT1.vhdx output.raw`

## Mount a vhdx disk to inspect the contents

`guestmount --add disk-image.vhdx --inspector --ro /mnt/`

## Copy image over internet with ssh an dd to a disk

`dd status=progress if=output.raw bs=1M | ssh root@1.1.1.1 dd of=/dev/vda`

## Copy image over internet with ssh an dd to a file

`dd status=progress if=/dev/vda bs=1M | ssh user@anderemachine dd of=/pad/naar/disk.raw`

You can add `conv=sparse` to dd command on the receiving machine.

## Make a backup of a filesytem over ssh

```bash
sudo tar zc / | ssh user@host cat - \> /path/to/tarball.tar.gz
```

```bash
#!/bin/bash
VBoxManage unregistervm db01 --delete
cd ~/VirtualBox\ VMs/ && rm -rf db01/


VBoxManage createvm --name db01 --ostype Ubuntu_64 --register
VBoxManage modifyvm db01 --memory 1024
VBoxManage modifyvm db01 --nic1 intnet --macaddress1 080027498558
VBoxManage modifyvm db01 --intnet1 'intnet'
VBoxManage modifyvm db01 --boot1 disk --boot2 net
VBoxManage createmedium --filename ~/VirtualBox\ VMs/db01/bootdisk.vdi --size 10000
VBoxManage createmedium --filename ~/VirtualBox\ VMs/db01/datadisk.vdi --size 10000
VBoxManage createmedium --filename ~/VirtualBox\ VMs/db01/datadisk2.vdi --size 10000
VBoxManage storagectl db01 --name "SATA" --add sata --controller IntelAHCI
VBoxManage storageattach db01 --storagectl "SATA" --port 0 --device 0 --type hdd --medium ~/VirtualBox\ VMs/db01/bootdisk.vdi
VBoxManage storageattach db01 --storagectl "SATA" --port 1 --device 0 --type hdd --medium ~/VirtualBox\ VMs/db01/datadisk.vdi
VBoxManage storageattach db01 --storagectl "SATA" --port 2 --device 0 --type hdd --medium ~/VirtualBox\ VMs/db01/datadisk2.vdi
```
