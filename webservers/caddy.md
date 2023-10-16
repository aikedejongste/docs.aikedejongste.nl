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

## Caddyfile with auth

```
mon.aike.be {
  log {
    output stdout
  }
  handle /loki* {
     reverse_proxy http://loki:3100
     basicauth /loki/* {
       {$LOKI_USER} {$LOKI_PASS}
     }
  }
  handle {
    reverse_proxy http://grafana:3000
  }
}
```
