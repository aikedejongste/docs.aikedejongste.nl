---
layout: default
title: Ubiquiti
has_children: false
parent: Networking
---

# Ubiquiti

## Install

```yaml
---
version: "3"
services:
  unifi-network-application:
    image: lscr.io/linuxserver/unifi-network-application:latest
    container_name: unifi-network-application
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MONGO_USER=unifi
      - MONGO_PASS=blablabla
      - MONGO_HOST=unifi-db
      - MONGO_PORT=27017
      - MONGO_DBNAME=unifi
      - MEM_LIMIT=1024
      - MEM_STARTUP=1024
      #- MONGO_TLS= #optional
      #- MONGO_AUTHSOURCE= #optional
    volumes:
      - /opt/unifi/data:/config
    ports:
      - 8443:8443
      - 3478:3478/udp
      - 10001:10001/udp
      - 8080:8080
      - 1900:1900/udp #optional
      - 8843:8843 #optional
      - 8880:8880 #optional
      - 6789:6789 #optional
      - 5514:5514/udp #optional
    restart: unless-stopped
  unifi-db:
    image: docker.io/mongo:4.4.18
    container_name: unifi-db
    volumes:
      - /opt/unifi/db:/data/db
      - /opt/unifi/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro
    restart: unless-stopped
  watchtower:
    image: containrrr/watchtower
    command: --run-once unifi-network-application
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      #- /root/.docker/config.json:/config.json
```
