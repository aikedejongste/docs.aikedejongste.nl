---
layout: default
title: Helm
has_children: true
---

# Helm

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
