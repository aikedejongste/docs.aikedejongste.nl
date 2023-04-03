---
layout: default
title: Cert-Manager
has_children: false
parent: Kubernetes
---

# Cert Manager

```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.11.0 \
  --set installCRDs=true
```
