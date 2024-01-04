---
layout: default
title: UFW
parent: Networking
---

# UFW

## UFW show rules when inactive

```bash
ufw show added
```

## UFW allow APP Profile from IP

Put quotes around app profiles with spaces.

```bash
ufw allow from 1.2.3.4/24 to any app <profile name>
```


## UFW without Ansible

```bash
ufw allow from 1.2.3.4 to any port ssh comment 'Aikes devjump host'
```

## UFW disable logging

```ufw logging off```

## UFW reset rules

```ufw reset```

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
