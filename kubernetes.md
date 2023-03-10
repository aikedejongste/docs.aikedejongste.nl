---
layout: default
title: Kubernetes
has_children: true
---

# Kubernetes

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

