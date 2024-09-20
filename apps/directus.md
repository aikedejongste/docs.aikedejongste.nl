---
layout: default
title: Directus
has_children: false
parent: Self Hosted Apps
---

# Directus

-  docker network create internal
-  put content below in /opt/compose.yaml
-  - set caddy label to your hostname
-  - set email
-  - set password
-  - set secret
-  - set public hostname
-  docker compose up -d

```yaml
services:
  caddy:
    image: lucaslorentz/caddy-docker-proxy:ci-alpine
    ports:
      - 80:80
      - 443:443
    environment:
      - CADDY_INGRESS_NETWORKS=internal
    networks:
      - internal
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./caddy_data:/data
    restart: unless-stopped

  database:
    image: postgis/postgis:13-master
    volumes:
      - ./data/database:/var/lib/postgresql/data
    networks:
      - internal
    environment:
      POSTGRES_USER: "directus"
      POSTGRES_PASSWORD: "directus"
      POSTGRES_DB: "directus"

  cache:
    image: redis:6
    networks:
      - internal

  directus:
    image: directus/directus:10.13.0
    labels:
      caddy: REPLACEME -> cms.company.com
      caddy.reverse_proxy: "{{upstreams 8055}}"
    volumes:
      - ./uploads:/directus/uploads
      - ./extensions:/directus/extensions
    networks:
      - internal
    depends_on:
      - cache
      - database
    environment:
      SECRET: "....."
      CORS_ENABLED: "true"
      CORS_ORIGIN: "true"

      DB_CLIENT: "pg"
      DB_HOST: "database"
      DB_PORT: "5432"
      DB_DATABASE: "directus"
      DB_USER: "directus"
      DB_PASSWORD: "directus"

      CACHE_ENABLED: "true"
      CACHE_AUTO_PURGE: "true"
      CACHE_STORE: "redis"
      REDIS: "redis://cache:6379"

      ADMIN_EMAIL: "......"
      ADMIN_PASSWORD: ".........."

      PUBLIC_URL: "https://cms.company.com"

networks:
  internal:
    external: true
```
