---
layout: default
title: Playbook Basics
parent: Ansible
---

# Playbook Basics

For servers that are kind of like snowflakes but you want some structure and still keep changes in git. 

```yaml
---
- name: Update apt-get repo and cache
  apt: update_cache=yes force_apt_get=yes cache_valid_time=3600

- name: Upgrade all apt packages
  apt: upgrade=dist force_apt_get=yes

- name: Install Packages
  apt:
    pkg:
      - open-iscsi
      - apt-transport-https
      - docker-compose
      - docker.io
    state: present
    update_cache: no

- name: Add the user 'Aike de Jongste'
  ansible.builtin.user:
    name: aikedejongste
    shell: /bin/bash
    groups: sudo,docker
    append: yes

- name: Set authorized keys taken from url
  ansible.posix.authorized_key:
    user: aikedejongste
    state: present
    key: https://github.com/aikedejongste.keys

- name: Make users passwordless for sudo in group sudo
  lineinfile:
    path: /etc/sudoers
    state: present
    regexp: '^%sudo'
    line: '%sudo ALL=(ALL) NOPASSWD: ALL'
    validate: 'visudo -cf %s'

```
