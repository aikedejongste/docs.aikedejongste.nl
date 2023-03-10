---
layout: default
title: IPtables with Ansible
parent: Networking
---

# IPtables with Ansible

## Useful links

* [Ansible docs](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/iptables_module.html)

## Set ip_forward=1 in systctl

```yaml
- ansible.posix.sysctl:
    name: net.ipv4.ip_forward
    value: '1'
    sysctl_set: true
    state: present
    reload: true
```

```yaml
- name: Create Iptables NAT chain
  iptables:
    table: nat
    chain: POSTROUTING
    out_interface: '{{ masquerade_out_interface }}'
    source: '{{ masquerade_source }}'
    destination: '{{ masquerade_destination }}'
    jump: MASQUERADE
    protocol: '{{ masquerade_protocol }}'
    comment: Ansible NAT Masquerade
```

