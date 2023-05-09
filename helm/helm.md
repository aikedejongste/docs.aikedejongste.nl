---
layout: default
title: Helm
has_children: true
---

# Helm

## Install Helm with APT

```bash
apt-get install gpg curl apt-transport-https --yes
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update && sudo apt-get install helm
```

## Install Helm with APT and Ansible

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

## Useful repos:

```bash
helm repo add jetstack https://charts.jetstack.io  # for cert-manager
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
