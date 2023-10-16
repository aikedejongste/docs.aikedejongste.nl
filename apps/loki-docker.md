---
layout: default
title: Loki on Docker
has_children: false
parent: Self Hosted Apps
---

# Loki Docker

## Tips

- Chown data dirs to 10001:10001?
- [Loki Docker driver](https://docs.aikedejongste.nl/logging/loki.md)
- [Caddy auth config here](https://docs.aikedejongste.nl/webservers/caddy.md)
- Hash passwords for Caddy with `caddy hash-password`

## Run with a docker-compose.yaml:

```yaml
version: "3"

networks:
  loki:

services:
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "443:443"
      - "443:443/udp"
    volumes:
      - /opt/Caddyfile:/etc/caddy/Caddyfile
      - /opt/caddy_data:/data
      - /opt/caddy_config:/config
    environment:
      - LOKI_USER=loki
      - LOKI_PASS= < user double $ to escape $ in password hashes here - create with caddy hash-password >
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
      - /opt/grafana_config/grafana.ini:/etc/grafana/grafana.ini
      - /opt/grafana_data:/var/lib/grafana
    networks:
      - loki
  loki:
    image: grafana/loki:latest
    command: -log.level=info -config.file=/etc/loki/local-config.yaml
    container_name: loki
    restart: always
    ports:
      - "3100:3100"
    volumes:
      - /opt/loki-data:/loki
      - /opt/loki-config/local-config.yaml:/etc/loki/local-config.yaml
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


## Very minimal Loki config:

```yaml
auth_enabled: false

server:
  http_listen_port: 3100

common:
  ring:
    instance_addr: 127.0.0.1
    kvstore:
      store: inmemory
  replication_factor: 1
  path_prefix: /loki

schema_config:
  configs:
  - from: 2020-05-15
    store: boltdb-shipper
    object_store: filesystem
    schema: v11
    index:
      prefix: index_
      period: 24h

storage_config:
  tsdb_shipper:
    active_index_directory: /loki/index
    cache_location: /loki/index_cache
    shared_store: filesystem
  filesystem:
    directory: /loki/chunks
```

## Other working Loki config:

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

## The Promtail config:

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push
    #basic_auth:
    #  username: abc
    #  password: 123

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

