---
layout: default
title: Docker Login
parent: Docker
has_children: false
---

# Docker Login

## With Ansible

```yaml
---
- name: Set Docker credentials for all users
  hosts: one, two, three
  become: yes
  vars_prompt:
    - name: docker_username
      prompt: "Enter Docker username"
      private: no
    - name: docker_token
      prompt: "Enter Docker token"
      private: yes

  tasks:
    - name: Get all user directories in /home
      command: ls /home
      register: users

    - name: Create .docker directory for each user
      file:
        path: "/home/{{ item }}/.docker"
        state: directory
        owner: "{{ item }}"
        group: "{{ item }}"
        mode: '0700'
      loop: "{{ users.stdout_lines }}"

    - name: Set Docker credentials for each user
      copy:
        content: |
          {
            "auths": {
              "https://ghcr.io": {
                "auth": "{{ (docker_username + ':' + docker_token) | b64encode }}"
              }
            }
          }
        dest: "/home/{{ item }}/.docker/config.json"
        owner: "{{ item }}"
        group: "{{ item }}"
        mode: '0600'
      loop: "{{ users.stdout_lines }}"
```
