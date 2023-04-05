---
layout: default
title: Roundcube
has_children: false
parent: Self Hosted Apps
---

# Roundcube

This Helm Chart works: https://artifacthub.io/packages/helm/mlohr/roundcube

Database port is required when using MySQL, but the chart doesn't check for that and sets the wrong default.

```bash
helm repo add mlohr https://helm-charts.mlohr.com/
helm repo update
```
