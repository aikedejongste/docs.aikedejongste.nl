---
layout: default
title: Docker Plugins
parent: Docker
has_children: false
---

# Docker Plugins

## Offline installation

### Install plugin on host with internet

```bash
docker plugin install grafana/loki-docker-driver:2.9.1 --alias loki --grant-all-permissions
```

### Compress the plugins and copy to target host

```bash
cd /var/lib/docker/plugins
tar --use-compress-program="pigz -k " -cf 652b55529f538b.....pigz 652b55529f538b........
```

### Extract on target host

```bash
cd /var/lib/docker/plugins
tar -I pigz -xvf 652b55529f538b.....pigz
```

### Restart Docker service and verify

```bash
systemctl restart docker && docker plugin ls
```
