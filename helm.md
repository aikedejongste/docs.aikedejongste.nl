---
layout: default
title: Helm
has_children: true
---

# Helm

## Install Helm with APT

```yaml
- name: Add key without using apt-key
  block:
    - name: download key
      ansible.builtin.get_url:
        url: https://baltocdn.com/helm/signing.asc
        dest: /etc/apt/trusted.gpg.d/helm.asc

    - name: GPG dearmor
      shell: cat /etc/apt/trusted.gpg.d/helm.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null

    - name: Add APT source
      ansible.builtin.apt_repository:
        repo: "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/helm.asc] https://baltocdn.com/helm/stable/debian/ all main"
        state: present

- name: Install Helm
  apt:
    pkg:
      - helm
    state: present
    update_cache: yes
```

## Get values from a chart in a repo
```bash
helm show values bitnami/matomo
```

## Rollout deployment with MultiAttach error fix

```yaml
updateStrategy:                                                                 
  type: RollingUpdate                                                           
  rollingUpdate:                                                                
    maxSurge: 0                                                                 
    maxUnavailable: 1 
```
