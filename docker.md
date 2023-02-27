---
layout: default
title: Docker
has_children: false
---

# Docker

## Remove all containers from a host:
```bash
docker rm -f $(docker ps -qa)
```

## Login with PAT

Create a new Personal Access Token [here](https://github.com/settings/tokens/new).

```bash
echo "ghp_REPLACE_ME" | docker login ghcr.io --username doesnt@matter.com --password-stdin
```

## Filter docker ps columns:

Link: [docker docs](https://docs.docker.com/engine/reference/commandline/ps/#format)

```
docker ps -a --format="table {{.Names}}\t{{.Ports}}\t{{.CreatedAt}}\t{{.Status}}\t{{.Mounts}}"
```
