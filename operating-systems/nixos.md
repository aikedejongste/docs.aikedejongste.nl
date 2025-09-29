---
layout: default
title: NixOS
has_children: false
parent: Operating Systems
---

# NixOS

## Installation on Hetzner Cloud

```yaml
#cloud-config

runcmd:
  - curl https://raw.githubusercontent.com/elitak/nixos-infect/master/nixos-infect | PROVIDER=hetznercloud NIX_CHANNEL=nixos-24.05 bash 2>&1 | tee /tmp/infect.log
```

## After boot

```bash
nix-shell -p git
git@gitlab.com:aikedejongste/nix.git && cd nix


```

sudo mv /etc/nixos /etc/nixos.bak  # Backup the original configuration
sudo ln -s ~/nixos-config/ /etc/nixos

# Deploy the flake.nix located at the default location (/etc/nixos)

sudo nixos-rebuild switch

## Useful commands

```bash
nixos-generate-config

nixos-rebuild switch --option eval-cache false --flake .#ocd4

You can always try to add --show-trace --print-build-logs --verbose to the nixos-rebuild command to get the detailed error message if you encounter any errors during the deployment. e.g.



## For Hetzner Cloud-init install
  boot.loader.grub.device = "nodev";

