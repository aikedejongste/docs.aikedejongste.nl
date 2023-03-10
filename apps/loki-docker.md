---
layout: default
title: Loki on Docker
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
    container_name: loki
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
    container_name: grafana
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
  promtail:
    container_name: promtail
    image: grafana/promtail:latest
    volumes:
      - /var/log:/var/log
      - ./promtail-config.yaml:/etc/promtail/config.yml
    command: -config.file=/etc/promtail/config.yml
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


The Promtail config:


```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
  - job_name: system
    static_configs:
    - targets:
        - localhost
      labels:
        job: varlogs
        __path__: /var/log/*log
  - job_name: apache
    static_configs:
    - targets:
        - localhost
      labels:
        job: apache
        __path__: /var/log/apache2/*log
  - job_name: journal
    journal:
      labels:
        cluster: cluster
        job: default/systemd-journal
      path: /var/log/journal
    relabel_configs:
    - source_labels:
      - __journal__systemd_unit
      target_label: systemd_unit
    - source_labels:
      - __journal__hostname
      target_label: nodename
    - source_labels:
      - __journal_syslog_identifier
      target_label: syslog_identifier

```
