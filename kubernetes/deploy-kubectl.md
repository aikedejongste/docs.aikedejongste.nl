---
layout: default
title: Deploy - kubectl
has_children: false
parent: Kubernetes
---

# Deploy - kubectl

## in Github Actions Workflow

Create the K8SPRODCRET with:

```bash
cat kubeconfig | base64 -w 0 
```

```yaml
      - name: Create kube dir for credentials
        run: mkdir -p $HOME/.kube

      - name: Save k8s credentials
        run: echo "${{ secrets.K8SPRODCERT }}" | base64 -d > $HOME/.kube/config

      - name: Deploy new version
        run: kubectl rollout restart deployment/portal -n portal

      - name: Shred k8s credentials
        run: shred $HOME/.kube/config
```
