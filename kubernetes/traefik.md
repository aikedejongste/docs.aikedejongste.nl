---
layout: default
title: Traefik dashboard k3s
has_children: false
parent: Kubernetes
---

# Traefik dashboard k3s

Dont forget to replace the hostname in the yaml below

```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: traefik-dashboard
  namespace: kube-system
spec:
  entryPoints:
    - web           # <-- using the web entrypoint, not the traefik (9000) one
  routes:           # v-- adding a host rule
    - match: Host(`traefik.aike.be`) && (PathPrefix(`/dashboard`) || PathPrefix(`/api`))
      kind: Rule
      services:
        - name: api@internal
          kind: TraefikService
```
