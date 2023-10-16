---
layout: default
title: Grafana
has_children: false
parent: Self Hosted Apps
---

# Grafana Docker

With a docker-compose.yaml:

```yaml
version: "3"

networks:
  internal:

services:
  grafana:
    container_name: grafana
    image: grafana/grafana:latest
    restart: always
    user: "472"
    ports:
      - "3000:3000"
    volumes:
      - /opt/grafana-config/grafana.ini:/etc/grafana/grafana.ini
      - /opt/grafana-data:/var/lib/grafana
    networks:
      - internal
```

## Permissions

```bash
chown -R 472:472 /opt/grafana*
```

## Configuration

```bash
docker cp grafana:/etc/grafana/grafana.ini grafana-config/grafana.ini
```

Set domain:

```ini
domain = grafana.aike.be
enforce_domain = false
root_url = %(protocol)s://%(domain)s:%(http_port)s/
```

## Vhost / reverse proxy
