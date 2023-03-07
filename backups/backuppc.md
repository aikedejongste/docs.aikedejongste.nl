---
layout: default
title: BackupPC
parent: Backups
---

# BackupPC

compose file:

```yaml
version: '3.7'
services:
  caddy:
    container_name: caddy
    image: caddy:latest
    restart: unless-stopped
    ports:
      - "443:443"
      - "443:443/udp"
    volumes:
      - /opt/backuppc/Caddyfile:/etc/caddy/Caddyfile
      - /opt/backuppc/caddy_data:/data
      - /opt/backuppc/caddy_config:/config
    networks:
      - proxy

  backuppc-app:
    image: tiredofit/backuppc:5.3.14
    container_name: backuppc-app
    volumes:
      - /var/lib/backuppc:/var/lib/backuppc
      - ./conf/etc/:/etc/backuppc
      - ./conf/home/:/home/backuppc
      - ./logs:/www/logs
    environment:
      - CONTAINER_NAME=backuppc-app
      - BACKUPPC_UUID=10000
      - BACKUPPC_GUID=10000
      - NGINX_AUTHENTICATION_TYPE=BASIC
      - NGINX_AUTHENTICATION_BASIC_USER1=backuppc
      - NGINX_AUTHENTICATION_BASIC_PASS1=whatever-something
      - DEBUG_MODE=FALSE
    networks:
      - proxy
    restart: always
    extra_hosts:
       - "some-hostname:1.2.3.4"
networks:
  proxy:
    external: true
```

Caddyfile:
```
backup.company.nl
reverse_proxy backuppc-app:80
tls /config/cert.pem /config/cert.key
```

