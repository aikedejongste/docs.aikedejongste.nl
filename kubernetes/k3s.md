---
layout: default
title: k3s
has_children: false
parent: Kubernetes
---

# K3s

## Internal database snapshots

K3s will automatically backup your embedded etcd datastore every 12 hours to /var/lib/rancher/k3s/server/db/snapshots/. You can reset the cluster by pointing to a specific snapshot.

## Install without Traefik

```bash
curl -sfL https://get.k3s.io | sh -s - --disable=traefik

k3s check-config
```

## Default registry??

In `/etc/rancher/k3s/registries.yaml`

```
mirrors:
  docker.io:
    endpoint:
      - "https://registry.gitlab.com"
configs:
  registry.gitlab.com:
    auth:
      username: you # this is the registry username
      password: 12345678 # this is the registry password
```
