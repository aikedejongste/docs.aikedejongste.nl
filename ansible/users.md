---
layout: default
title: Users
parent: Ansible
---

# Users

## Playbook

```yaml
---
- name: Configure Users
  hosts: all
  gather_facts: yes
  become: true
  roles:
    - { role: users,         tags: users }
  vars:
    users:
      - name: aikedejongste
        groups: sudo,docker,deploy
```

## Tasks:

```yaml
{% raw %}
---
- name: Extract unique groups
  set_fact:
    unique_groups: "{{ users | json_query('[].groups') | join(',') | split(',') | unique }}"

- name: Create groups
  ansible.builtin.group:
    name: "{{ item }}"
    state: present
  loop: "{{ unique_groups }}"

- name: Ensure deploy group has sudo privileges
  lineinfile:
    dest: /etc/sudoers
    state: present
    regexp: "^%deploy"
    line: "%deploy ALL=(ALL) NOPASSWD:ALL"
    validate: "/usr/sbin/visudo -cf %s"

- name: Add users
  ansible.builtin.user:
    name: "{{ item.name }}"
    shell: /bin/bash
    state: present
    groups: "{{ item.groups | default(omit) }}"
  with_items: "{{ users }}"

- name: Set up SSH keys
  ansible.builtin.authorized_key:
    user: "{{ item.name }}"
    state: present
    key: "{{ lookup('file', item.name + '.pub') }}"
  with_items: "{{ users }}"

{% endraw %}
```
