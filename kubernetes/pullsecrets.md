---
layout: default
title: Pullsecrets
has_children: false
parent: Kubernetes
---

# Pullsecrets

TODO: add encrypt with Ansible Vault steps

```yaml

vars:
  pullsecret: ....

- name: Apply Reflector
  kubernetes.core.k8s:
    kubeconfig: /root/kubeconfig
    state: present
    src: https://github.com/emberstack/kubernetes-reflector/releases/latest/download/reflector.yaml

- name: Create pullSecret Reflector
  kubernetes.core.k8s:
    kubeconfig: /root/kubeconfig
    state: present
    definition:
      kind: Secret
      apiVersion: v1
      metadata:
        namespace: default
        name: pullsecret-ghcr
        annotations:
          reflector.v1.k8s.emberstack.com/reflection-allowed: "true"
          reflector.v1.k8s.emberstack.com/reflection-auto-enabled: "true"
      type: kubernetes.io/dockerconfigjson
      data:
        .dockerconfigjson: "{{ pullsecret }}"
```
