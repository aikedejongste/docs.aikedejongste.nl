---
layout: default
title: Caddy Proxy
parent: Docker
---

# Docker Compose Caddy Proxy

## Very simple

```bash
docker network create internal
```

```yaml
version: '3.7'
services:

  caddy:
    image: lucaslorentz/caddy-docker-proxy:ci-alpine
    ports:
      - 80:80
      - 443:443
    environment:
      - CADDY_INGRESS_NETWORKS=internal
    networks:
      - caddy
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - caddy_data:/data
    restart: unless-stopped

  app:
    image: traefik/whoami
    networks:
      - internal
    labels:
      caddy: www.portpatrol.app
      caddy.reverse_proxy: "{{upstreams 80}}"

volumes:
  caddy_data: {}

networks:
  internal:
    external: true
```


