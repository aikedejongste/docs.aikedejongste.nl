---
layout: default
title: Loki Docker
has_children: false
parent: Self Hosted Apps
---

# Loki Docker

With a docker-compose.yaml:

```yaml
version: "3"

networks:
  loki:

services:
  loki:
    image: grafana/loki:latest
    restart: always
    ports:
      - "3100:3100"
    command: -config.file=/etc/loki/local-config.yaml
    volumes:
      - /opt/loki/loki-data:/loki
      - /opt/loki/loki-config.yaml:/etc/loki/local-config.yaml
    networks:
      - loki
  grafana:
    image: grafana/grafana:latest
    restart: always
    user: "472"
    ports:
      - "3000:3000"
    volumes:
      - ./grafana-config.ini:/etc/grafana/grafana.ini
      - ./grafana-data:/var/lib/grafana
    networks:
      - loki
```

The Loki config:

```yaml

auth_enabled: false
server:
  http_listen_port: 3100

common:
  path_prefix: /loki

ingester:
  lifecycler:
    address: 127.0.0.1
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1
    final_sleep: 0s
  chunk_idle_period: 5m
  chunk_retain_period: 30s

schema_config:
  configs:
  - from: 2020-10-24
    store: boltdb-shipper
    object_store: filesystem
    schema: v12
    index:
      prefix: index_
      period: 24h

storage_config:
  boltdb:
    directory: /loki/index
  filesystem:
    directory: /loki/chunks

limits_config:
  ingestion_burst_size_mb: 64
  ingestion_rate_mb: 32
  max_streams_per_user: 100000
  retention_period: 100d
```
