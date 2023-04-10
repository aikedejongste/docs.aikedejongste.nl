---
layout: default
title: Docker
has_children: true
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

```bash
{% raw %} docker ps -a --format="table {{.Names}}\t{{.Ports}}\t{{.CreatedAt}}\t{{.Status}}\t{{.Mounts}}" {% endraw %}
```

## Watchdower for automatic updates

```yaml
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /root/.docker/config.json:/config.json
```

## Mount NFS in docker-compose file

Go to [NFS page](linux/nfs.html)


## Docker-compose

```yaml
networks:
  frontend:
  backend:
```
