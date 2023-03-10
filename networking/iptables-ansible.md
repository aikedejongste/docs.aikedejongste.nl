---
layout: default
title: IPtables with Ansible
parent: Networking
---

# IPtables with Ansible

## Set ip_forward=1 in systctl

```yaml
- ansible.posix.sysctl:
    name: net.ipv4.ip_forward
    value: '1'
    sysctl_set: true
    state: present
    reload: true
```
