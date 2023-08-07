---
layout: default
title: Caddy
parent: Webservers
---

# Caddy

## Simple proxy pass

```
  caddy:
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

```

## Caddyfile

```
backup.company.nl

reverse_proxy backuppc-app:80

tls /config/cert.pem /config/cert.key
```
