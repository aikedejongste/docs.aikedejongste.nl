---
layout: default
title: Kubernetes
has_children: true
---

# Kubernetes


## Install k3s SINGLE NODE

```yaml
curl -sfL https://get.k3s.io | sh - 
k3s kubectl get node 
```

## Install k3s CLUSTER (3 nodes)

```yaml
# on node1
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server --cluster-init
# on the other 2 nodes
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server --server https://<ip or hostname of server1>:6443

k3s kubectl get node 
```

## Copy file from local to pod:

```bash
kubectl cp config.ini.php matomo/matomo-5b5c6c758d-9wdvm:/bitnami/matomo/config/config.ini.php
```

## K3s install with forced private ip

Source: [Github](https://github.com/alexellis/k3sup/issues/306#issuecomment-1059986048)

```bash
k3sup install --ip server_ip--user ubuntu --k3s-extra-args '--node-external-ip server_ip --node-ip server_ip'
k3sup join --user ubuntu --server-ip server_ip --ip agent_ip
```

## K3s examples for Hetzner Cloud

* [https://github.com/StarpTech/k-andy](https://github.com/StarpTech/k-andy)
* [https://github.com/cicdteam/terraform-hcloud-k3s](https://github.com/cicdteam/terraform-hcloud-k3s)

