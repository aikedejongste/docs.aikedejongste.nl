---
layout: default
title: Proxmox
parent: Virtualization
has_children: false
---

# Proxmox

## Resolve locked vm problem

```bash
qm unlock 105

rm /var/lock/qemu-server/lock-105.conf

qm stop 105

```

## Required for Windows nested virt

```bash
echo "options kvm ignore_msrs=1" >> /etc/modprobe.d/kvm.conf
```

## Create a Windows 11 vm with nested virtualization

First version of script that creates Win 11 vm's.

TODO: dir for iso
TODO: stop on error
TODO: check if vm exists
TODO: add networking (after Windows install)

```bash
#!/bin/bash

VM_ID=110
VM_NAME=win11-lungmuss3
ISO_STORE="nfs-isos"
WIN_ISO="Win11_22H2_EnglishInternational_x64v1.iso"
VMGEN_ID=$(uuidgen)
MAC_ADDRESS=$(printf '02:%02x:%02x:%02x:%02x:%02x' $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)))


qm create $VM_ID --name $VM_NAME
qm set $VM_ID --tpmstate0 local-lvm:4,version=v2.0
qm set $VM_ID --efidisk0 local-lvm:4,efitype=4m,pre-enrolled-keys=1
qm set $VM_ID --ostype win11
qm set $VM_ID --bios ovmf
qm set $VM_ID --sockets 1
qm set $VM_ID --cores 4
qm set $VM_ID --cpu host,flags=+hv-evmcs
qm set $VM_ID --machine pc-q35-8.0
qm set $VM_ID --memory 8192
qm set $VM_ID --ide2 $ISO_STORE:iso/virtio-win.iso,media=cdrom,size=522284K
qm set $VM_ID --scsi0 local-lvm:100
qm set $VM_ID --sata0 $ISO_STORE:iso/Win11_22H2_EnglishInternational_x64v1.iso,media=cdrom,size=5418024K
qm set $VM_ID --scsihw virtio-scsi-pci
qm set $VM_ID --numa 0
qm set $VM_ID --vmgenid $VMGEN_ID
qm set $VM_ID --vmgenid $VMGEN_ID
#qm set $VM_ID --agent=1,fstrim_cloned_disks=1
#qm set $VM_ID  --boot "order=scsi0;sata0;ide2"

echo "args: -cpu Cooperlake,hv_relaxed,hv_spinlocks=0x1fff,hv_vapic,hv_time,+vmx" >> "/etc/pve/qemu-server/$VM_ID.conf"

echo "On the “Let’s connect you to a network” page, use the “Shift + F10” keyboard shortcut. In Command Prompt, type the OOBE\BYPASSNRO command to bypass network requirements on Windows 11 and press Enter."

read -p "Install Windows now and press space to continue."

qm set $VM_ID --net0 e1000="$MAC_ADDRESS",bridge=vmbr0

echo "VM setup complete."
```


