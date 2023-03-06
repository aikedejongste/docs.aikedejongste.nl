---
layout: default
title: Ansible
#nav_order: 50
has_children: true
#permalink: /docs/ui-component
---

# Ansible

## Install Ansible on Ubuntu

```bash
apt-get update && \
  apt-get install -y gcc python-dev libkrb5-dev && \
  apt-get install python3-pip -y && \
  pip3 install --upgrade pip && \
  pip3 install --upgrade virtualenv && \
  pip3 install ansible
```

