---
layout: default
title: k3s
has_children: false
parent: Kubernetes
---

# K3s


## Internal database snapshots

K3s will automatically backup your embedded etcd datastore every 12 hours to /var/lib/rancher/k3s/server/db/snapshots/. You can reset the cluster by pointing to a specific snapshot.

