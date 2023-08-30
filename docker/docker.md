---
layout: default
title: Docker
has_children: true
---

# Docker

## Install

```bash
apt install -y apparmor-utils docker.io docker-compose
```

## SSH in Dockerfile (ugly!)

RUN mkdir -p -m 0600 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
RUN ssh-agent sh -c 'echo "$SSH_KEY" | ssh-add - ; npm install --force'

## Remove all containers from a host

```bash
docker rm -f $(docker ps -qa)
```

## Login with PAT

Create a new Personal Access Token [here](https://github.com/settings/tokens/new).

```bash
export PAT=.....
echo $PAT | docker login ghcr.io --username doesnt@matter.com --password-stdin
```

or

```bash
echo "ghp_REPLACE_ME" | docker login ghcr.io --username doesnt@matter.com --password-stdin
```

## Filter docker ps columns

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

<!-- markdown-link-check-disable -->
Go to [NFS page](linux/nfs.html)
<!-- markdown-link-check-enable -->

## Docker-compose

```yaml
networks:
  frontend:
  backend:
```

## Run sleep in a container that won't start

```bash
docker run --rm --entrypoint sh ubuntu:latest -c "sleep infinity"
```
