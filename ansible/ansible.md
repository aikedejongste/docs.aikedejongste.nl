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
  apt-get install -y gcc python3-dev libkrb5-dev && \
  apt-get install python3-pip -y && \
  pip3 install --upgrade pip && \
  pip3 install --upgrade virtualenv && \
  pip3 install ansible
```

## Display message

```yaml
    - name: Display PG instruction
      command: echo "PGPASSWORD=blabbalbala psql -U postgres -h lb-ip-goes-here -p 5432"
      register: instruction

    - debug: msg="{{ instruction.stdout }}"
```

