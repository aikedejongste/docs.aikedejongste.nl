---
layout: default
title: UFW
parent: Networking
---

# UFW

## UFW disable logging
```ufw logging off```

## UFW disable logging with Ansible
```yaml
- name: UFW disable logging
  community.general.ufw:
    logging: off
```

## Other UFW with Ansible

```yaml
- name: Allow HTTP from everywhere
  community.general.ufw:
    rule: allow
    port: 80

- name: Allow HTTPS from everywhere
  community.general.ufw:
    rule: allow
    port: 443

- name: Allow SSH from selected hosts
  community.general.ufw:
    rule: allow
    port: '22'
    src: '{{ item }}'
  loop:
    - 1.2.3.4/32 # Aikes devjump host

- name: Outgoing allow
  community.general.ufw:
    state: enabled
    direction: outgoing
    policy: allow

- name: Incoming deny
  community.general.ufw:
    state: enabled
    direction: incoming
    policy: deny

- name: UFW disable logging
  community.general.ufw:
    logging: off
```
