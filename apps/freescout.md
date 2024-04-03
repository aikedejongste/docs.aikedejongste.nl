---
layout: default
title: FreeScout
has_children: false
parent: Self Hosted Apps
---

# FreeScout

## Compose file

TODO:

- [ ] put env vars in .env
- [ ] does Caddy need access to docker.sock?
- [ ] better networking

```
version: '3.7'

services:
  caddy:
    image: lucaslorentz/caddy-docker-proxy:ci-alpine
    ports:
      - 80:80
      - 443:443
    environment:
      - CADDY_INGRESS_NETWORKS=private
    networks:
      - private
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./caddy_data:/data
    restart: unless-stopped

  freescout-app:
    image: tiredofit/freescout
    container_name: freescout-app
    links:
    - freescout-db
    volumes:
    - ./data:/data
    ### If you want to just keep the original source and add additional modules uncomment this line
    #- ./modules:/www/html/Modules
    - ./logs/:/www/logs
    labels:
      caddy: support.you.com
      caddy.reverse_proxy: "{{upstreams}}"
    environment:
    - VIRTUAL_HOST=support.you.com
    - VIRTUAL_NETWORK=private
    - VIRTUAL_PORT=80
    - LETSENCRYPT_HOST=support.you.com
    - LETSENCRYPT_EMAIL=support.you.com

    - CONTAINER_NAME=freescout-app

    - DB_HOST=freescout-db
    - DB_NAME=freescout
    - DB_USER=freescout
    - DB_PASS=..........

    - SITE_URL=https://support.you.com
    - ADMIN_EMAIL=you@you.com
    - ADMIN_PASS=.........
    - ENABLE_SSL_PROXY=TRUE
    - DISPLAY_ERRORS=TRUE
    - TIMEZONE=Europe/Amsterdam
    networks:
      - private
    restart: always

  freescout-db:
    image: tiredofit/mariadb
    container_name: freescout-db
    volumes:
      - ./db:/var/lib/mysql
    environment:
      - ROOT_PASS=password
      - DB_NAME=freescout
      - DB_USER=freescout
      - DB_PASS=.......

      - CONTAINER_NAME=freescout-db
    networks:
      - private
    restart: always

  freescout-db-backup:
    container_name: freescout-db-backup
    image: tiredofit/db-backup
    links:
     - freescout-db
    volumes:
      - ./dbbackup:/backup
    environment:
      - CONTAINER_NAME=freescout-db-backup
      - DB_HOST=freescout-db
      - DB_TYPE=mariadb
      - DB_NAME=freescout
      - DB_USER=freescout
      - DB_PASS=.........
      - DB01_BACKUP_INTERVAL=1440
      - DB01_BACKUP_BEGIN=0000
      - DB_CLEANUP_TIME=8640
      - COMPRESSION=BZ
      - MD5=TRUE
    networks:
      - private
    restart: always


networks:
  private:
    external: true
```
