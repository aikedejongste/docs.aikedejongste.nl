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

The out_interface is usually the public interface!

```yaml
- name: Get default route
  shell: ip route | grep default | awk '{print $5}'
  register: public_interface

- name: Print public interface
  debug:
    msg: "Public interface is {{ public_interface.stdout }}"

- name: Create Iptables NAT chain
  iptables:
    table: nat
    chain: POSTROUTING
    out_interface: '{{ public_interface.stdout }}'
    source: '{{ masquerade_source }}'
    destination: '{{ masquerade_destination }}'
    jump: MASQUERADE
    protocol: '{{ masquerade_protocol }}'
    comment: Ansible NAT Masquerade
  vars:
    masquerade_source: '10.10.1.0/24'                                                                                                                                                                              masquerade_destination: '0.0.0.0/0'
    masquerade_jump: MASQUERADE
    masquerade_protocol: 'all'
```

## iptables-persistent to store current rules to file

```yaml
- name: Save current state of the firewall in system file
  community.general.iptables_state:
    ip_version: ipv4
    state: saved
    path: /etc/iptables/rules.v4
```

## Optional steps:

```yaml
- name: Set ip forwarding on in /proc and in the sysctl file and reload if necessary
  ansible.posix.sysctl:
    name: net.ipv4.ip_forward
    value: '1'
    sysctl_set: true
    state: present
    reload: true

- name: Allow related and established connections
  ansible.builtin.iptables:
    chain: INPUT
    ctstate: ESTABLISHED,RELATED
    jump: ACCEPT

- name: Allow new incoming SYN packets on TCP port 22 (SSH)
  ansible.builtin.iptables:
    chain: INPUT
    protocol: tcp
    destination_port: 22
    ctstate: NEW
    syn: match
    jump: ACCEPT
    comment: Accept new SSH connections.
```
