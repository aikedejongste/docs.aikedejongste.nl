---
layout: default
title: Rails - Docker Compose
has_children: false
parent: Programming
---

# Rails - Docker Compose

## Full example

```yaml
version: '3.3'

services:
  caddy:
    image: caddy:latest
    container_name: goproxy
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
  redis:
    image: redis:7
    container_name: redis
    restart: unless-stopped
    volumes:
      - ./redis_data:/data
    networks:
      - internal
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
  app:
    image: ....
    container_name: goprod
    restart: unless-stopped
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
    env_file:
      - ./.env
    volumes:
      - ./tmp:/rails/tmp
      - ./uploads:/rails/uploads
      - ./multivers.yml:/rails/config/multivers.yml
    networks:
      - internal
  workers:
    image: ....
    container_name: goworkers
    restart: unless-stopped
    command: bundle exec sidekiq -e production -C /rails/config/sidekiq.yml
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
    env_file:
      - ./.env
    volumes:
      - ./tmp:/rails/tmp
      - ./uploads:/rails/uploads
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
      - WATCHTOWER_HTTP_API_TOKEN=$WATCHTOWER_HTTP_API_TOKEN
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
    networks:
      - internal
  dbbackup:
    image: aikedejongste/db-dumper
    container_name: dbbackup
    volumes:
      - /var/backups:/mnt/target
    environment:
      PGHOST: $PGHOST
      PGUSER: $PGUSER
      PGPASSWORD: $PGPASSWORD
      MONITORURL: $MONITORURL
      SCHEDULE: $SCHEDULE
      #PGLIST: $PGLIST
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
    networks:
      - internal

networks:
  internal:

```
