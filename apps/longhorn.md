---
layout: default
title: Longhorn
has_children: false
parent: Self Hosted Apps
---

# Longhorn

## Traefik IngressRoute

```yaml
kind: IngressRoute
apiVersion: traefik.containo.us/v1alpha1
metadata:
  name: longhorn-frontend
  namespace: longhorn-system

spec:
  entryPoints: 
    - web
  routes:
    - match: Host(`{YOUR-DNS}`) 
      kind: Rule
      services:
        - name: longhorn-frontend
          port: 80
```

## Ansible install with Helm

```yaml
- name: Add helm repo                                                                                                                                                                                          
  kubernetes.core.helm_repository:                                                                                                                                                                             
    name: longhorn                                                                                                                                                                                             
    repo_url: "https://charts.longhorn.io"                                                                                                                                                                     
                                                                                                                                                                                                               
- name: Deploy Longhorn Helm Chart                                                                                                                                                                             
  kubernetes.core.helm:                                                                                                                                                                                        
    name: longhorn                                                                                                                                                                                             
    kubeconfig: /root/kubeconfig                                                                                                                                                                               
    chart_ref: longhorn/longhorn                                                                                                                                                                               
    release_namespace: longhorn-system                                                                                                                                                                         
    create_namespace: true
    update_repo_cache: true
    values_files:
      - longhorn-values.yaml
```

## Ansible steps without Helm

```
- name: Install Longhorn dependencies
  apt:
    pkg:
      - open-iscsi
    state: present
    update_cache: no

- name: Download Longhorn manifest to the cluster.
  ansible.builtin.get_url:
    url: https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/longhorn.yaml
    dest: /tmp/longhorn.yaml
    mode: '0664'

- name: Apply Longhorn manifest to the cluster.
  kubernetes.core.k8s:
    state: present
    src: /tmp/longhorn.yaml
    kubeconfig: /root/kubeconfig
```
