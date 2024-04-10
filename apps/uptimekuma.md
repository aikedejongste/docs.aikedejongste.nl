---
layout: default
title: UptimeKuma
has_children: false
parent: Self Hosted Apps
---

# UptimeKuma

[link](https://github.com/louislam/uptime-kuma)

## With Docker Compose


## With Kamal

```
uptimekuma:
  image: louislam/uptime-kuma:1
  host: www.yourcompany.com
  port: 3001
  container_name: uptimekuma
  directories:
    - /opt/uptime-kuma/data:/app/data
  labels:
    traefik.enable: true
    traefik.http.routers.uptimekuma_secure.service: yourcompany-uptimekuma@docker
    traefik.http.routers.uptimekuma_secure.entrypoints: websecure
    traefik.http.routers.uptimekuma_secure.rule: Host(`kuma.yourcompany.com`)
    traefik.http.routers.uptimekuma_secure.tls: true
    traefik.http.routers.uptimekuma_secure.tls.certresolver: letsencrypt
  options:
    network: "private"
```
