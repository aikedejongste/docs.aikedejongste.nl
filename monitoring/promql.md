---
layout: default
title: PromQL
has_children: false
parent: Monitoring
---

# Prometheus Query Language


## Queries

### Predict days until disk full

`(-node_filesystem_free_bytes{mountpoint="/",instance="host.you.app:9100"} / deriv(node_filesystem_free_bytes{mountpoint="/"}[24h])) / 3600 / 24`

### Percentage of failed requests for app

`traefik_service_requests_total{service="appname@docker",code="500"} / traefik_service_requests_total{service="appname@docker",code="200"} `

### Percentage of failed requests for everything

`sum(rate(traefik_service_requests_total{code=~"4.*"}[3m])) by (service) / sum(rate(traefik_service_requests_total[3m])) by (service) * 100`

