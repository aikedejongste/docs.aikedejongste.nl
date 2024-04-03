---
layout: default
title: Docker - Watchtower deploys
parent: Docker
has_children: false
---

# Watchtower deploys

## Schedule

On weekdays between 9:00 and 22:00 every 15 minutes.

```yaml
environment:
  - WATCHTOWER_SCHEDULE=0 */15 9-22 * * 1-5
```


## Manually trigger deploy

```bash
curl -H "Authorization: Bearer 12345678" https://host.company.nl/update/v1
```

## Compose file with 1 watched and 1 not

```yaml
version: '3.3'

services:
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - 0.0.0.0:443:443
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_data:/data
      - ./caddy_config:/config
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
    networks:
      - internal

  app:
    image: company/app:latest
    container_name: app
    restart: unless-stopped
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
    env_file:
      - ./.env
    networks:
      - internal

  deployer:
    image: containrrr/watchtower
    container_name: deployer
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /root/.docker/config.json:/config.json
    command: --debug --http-api-update
    environment:
      - WATCHTOWER_HTTP_API_TOKEN=12345678
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
    networks:
      - internal

networks:
  internal:

```
