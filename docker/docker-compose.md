---
layout: default
title: Docker Compose
parent: Docker
---

# Docker Compose

## force restart

```bash
docker-compose pull && docker-compose up -d --no-deps --force-recreate app
```

## Very simple

```yaml
version: '3.3'

services:
  caddy:
    image: caddy:latest
    restart: unless-stopped
    ports:
      - 0.0.0.0:443:443
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_data:/data
      - ./caddy_config:/config
  app:
    image: containous/whoami:latest
    restart: unless-stopped
```

## Simple example

```yaml
version: '3.3'

services:
  caddy:
    image: caddy:latest
    restart: unless-stopped
    ports:
      - 0.0.0.0:80:80
      - 0.0.0.0:443:443
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_data:/data
      - ./caddy_config:/config
    networks:
      - frontend
  backend:
    image: ......
    container_name: backend
    restart: always
    volumes:
      - /opt/backend/.env:/var/www/html/.env
    environment:
      MARIADB_ROOT_PASSWORD: ${MARIADB_ROOT_PASSWORD}
    networks:
      - frontend
      - backend
  db:
    image: docker.io/bitnami/mariadb:10.7
    container_name: db
    restart: always
    volumes:
      - /opt/mariadb_data:/bitnami/mariadb
    environment:
      - MARIADB_ROOT_PASSWORD=${MARIADB_ROOT_PASSWORD}
    healthcheck:
      test: ['CMD', '/opt/bitnami/scripts/mariadb/healthcheck.sh']
      interval: 15s
      timeout: 5s
      retries: 6
    networks:
      - backend

networks:
  frontend:
  backend:
```

## Watchtower for automatic updates

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
