---
layout: default
title: Sops and Age
has_children: false
---

# Encryption with Sops and Age

## Installation on Ubuntu

```bash
wget $(curl -s https://api.github.com/repos/mozilla/sops/releases/latest | jq -r '.assets[]' | grep amd64.deb | grep download | awk -F '"' '{print $4}') -O /tmp/sops.deb

sudo dpkg -i /tmp/sops.deb && sudo apt install -y age
```

## Create a keypair in ~/.sops

```bash
mkdir -p ~/.sops/ && age-keygen -o ~/.sops/age-key.txt && cat ~/.sops/age-key.txt
```

## Use an existing key:

```bash
mkdir -p ~/.sops/ && vim ~/.sops/age-key.txt
```
Fill it with:

```
# created: 2022-02-13T14:52:26Z
# public key: age12uxavhlq08kn7h66dd5ejwrg4dj7uhkzj0ac9ylzaxapmae73y3qt44djh
AGE-SECRET-KEY-1Y9FH6J9YPG964SZP5DRFGDQZ5L4Vasdlfjasdlkfjasldjfaskdfja
```

## Configure sops:

```bash
echo 'export SOPS_AGE_KEY_FILE=~/.sops/age-key.txt' >> ~/.bashrc
```

## Configure your repo or system

Put a `.sops.yaml` in your repo with the public key:

```yaml
creation_rules:
    - age: age12uxavhlq08kn7h66dd5ejwrg4dj7uhkzj0ac9ylzaxapmae73y3qt44djh
```

More advanced setups are possible, as well as system wide settings.



