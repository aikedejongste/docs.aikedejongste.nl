---
layout: default
title: Prometheus
has_children: false
parent: Monitoring
---

# Prometheus


```yaml

version: "3"
services:
  prometheus:
    image: prom/prometheus:latest
    user: 1000:1000
    volumes:
      - ./prometheus_config/:/etc/prometheus/
      - ./prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--storage.tsdb.retention.time=999d'
      - '--storage.tsdb.retention.size=10GB'
      - '--web.console.libraries=/usr/share/prometheus/console_libraries'
      - '--web.console.templates=/usr/share/prometheus/consoles'
      - '--web.enable-lifecycle'
    ports:
      - 9090:9090
 links:
   - alertmanager:alertmanager
 restart: always
alertmanager:
  image: quay.io/prometheus/alertmanager:latest
  user: 1000:1000
  volumes:
    - ./alertmanager_config:/etc/alertmanager/
    - ./alertmanager_data/:/alertmanager/
  command:
    - '--config.file=/etc/alertmanager/alertmanager.yml'
    - '--storage.path=/alertmanager'
  ports:
    - 9093:9093
  restart: never

```

## Queries

### Predict days until disk full:

`(-node_filesystem_free_bytes{mountpoint="/",instance="host.you.app:9100"} / deriv(node_filesystem_free_bytes{mountpoint="/"}[24h])) / 3600 / 24`
