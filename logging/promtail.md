---
layout: default
title: Promtail
parent: Logging
---

# Promtail

## docker-compose example

```yaml
version: "3"

services:
  promtail:
    image: grafana/promtail:latest
    container_name: promtail
    restart: always
    volumes:
      - /var/log:/var/log
      - /home/deploy/checkout/StekkerWeb/log:/var/log/rails
      - /opt/promtail/promtail.yml:/etc/promtail/config.yml
      - /var/log/journal/:/var/log/journal/
      - /run/log/journal/:/run/log/journal/
      - /etc/machine-id:/etc/machine-id
    command: -config.file=/etc/promtail/config.yml
```

## Promtail config

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: https://loki.company.name/loki/api/v1/push
    basic_auth:
      username: loki
      password: 123456789

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log


scrape_configs:
  - job_name: system
    static_configs:
    - targets:
        - https://loki.company.name
      labels:
        nodename: node-name
        job: varlogs
        __path__: /var/log/*log
  - job_name: apache
    static_configs:
    - targets:
        - https://loki.company.name
      labels:
        nodename: node-name
        job: apache
        __path__: /var/log/apache2/*log
  - job_name: rails
    static_configs:
    - targets:
        - https://loki.company.name
      labels:
        nodename: node-name
        job: rails
        __path__: /var/log/rails/*log
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
