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
  pip3 install ansible && \
  pip3 install jmespath
```

## Install modules/colletions

```bash
ansible-galaxy collection install kubernetes.core && \
ansible-galaxy collection install ansible.posix && \
ansible-galaxy collection install community.general && \
ansible-galaxy collection install community.sops && \
ansible-galaxy collection install community.postgresql
```

## Display message

```yaml
    - name: Display PG instruction
      command: echo "PGPASSWORD=blabbalbala psql -U postgres -h lb-ip-goes-here -p 5432"
      register: instruction

    - debug: msg="{{ instruction.stdout }}"
```

## Read file on local machine

```yaml
    - name: Read file contents
      set_fact:
        file_content: "{{ lookup('file', '/path/to/your/file') }}"

    - name: Display file contents
      debug:
        var: file_content
```

## Read file on remote machine

```yaml
    - name: Get file content from remote host
      slurp:
        src: /path/to/your/file
      register: file_content

    - name: Decode file content
      set_fact:
        file_content: "{{ file_content['content'] | b64decode }}"

    - name: Display file content
      debug:
        var: file_content
```
