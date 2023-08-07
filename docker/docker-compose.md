---
layout: default
title: Docker Compose
parent: Docker
---

# Docker Compose

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

## Docker bridge subnet

```
networks:
  appnetwork:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.16.0.0/24
          gateway: 172.16.0.1
```
