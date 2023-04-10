---
layout: default
title: VirtualBox
has_children: false
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



