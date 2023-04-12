---
layout: default
title: Playbook with prompt
parent: Ansible
---

# Playbook with prompt

```yaml
- name: rolename
  hosts: host1,host2
  become: yes
  roles:
    - { role: rolename, tags: tagname }
  vars_prompt:
    - name: Make sure you added a newline between certs
      prompt: Did you add a newline between certs and the cert bundle?
      default: "Yes"
```
