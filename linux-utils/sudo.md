---
layout: default
title: Sudo
has_children: false
parent: Linux
---

# Sudo

## Sysadmin user with sudo

```bash
adduser sysadmin --disabled-password --ingroup sudo && echo 'sysadmin ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
```

## Give yourself sudo permissions without password

```bash
echo "`whoami` ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/`whoami` && sudo chmod 0440 /etc/sudoers.d/`whoami`
```
