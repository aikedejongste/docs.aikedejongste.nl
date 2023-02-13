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

## Copy image over internet with ssh an dd

`dd status=progress if=output.raw bs=1M | ssh root@1.1.1.1 dd of=/dev/vda`
