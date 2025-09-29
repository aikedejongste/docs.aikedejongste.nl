---
layout: default
title: Prometheus
has_children: false
parent: Monitoring
---

# Prometheus

## Run with docker-compose

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

### Predict days until disk full

`(-node_filesystem_free_bytes{mountpoint="/",instance="host.you.app:9100"} / deriv(node_filesystem_free_bytes{mountpoint="/"}[24h])) / 3600 / 24`

## Build container with config with Github Actions

```yaml
name: BuildPrometheusContainer

on:
  workflow_dispatch:
  push:
    branches:
    - master
    paths: [ "infra/prometheus/*", ".github/workflows/prometheus.yaml" ]


env:
  REGISTRY: ghcr.io/your-company
  IMAGE_NAME: prometheus-your-company

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: GitHub Environment Variables Action
      uses: FranzDiebold/github-env-vars-action@v2

    - name: Log in to the Container registry
      uses: docker/login-action@65b78e6e13532edd9afa3aa52ac7964289d1a9c1
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - name: promtool check config /etc/prometheus/prometheus.yml
      run: docker run --rm -t -v "$(pwd)/infra/prometheus/:/etc/prometheus/" --entrypoint "/bin/promtool" prom/prometheus check config /etc/prometheus/prometheus.yml

    - name: Build the Docker image
      run: cd infra/prometheus && docker build . --label "org.opencontainers.image.source=https://github.com/your-company/${{ env.IMAGE_NAME }}" --tag ghcr.io/your-company/${{ env.IMAGE_NAME }}:latest --file Dockerfile

    - name: Docker push with latest tag
      run: docker push ghcr.io/your-company/${{ env.IMAGE_NAME }}:latest
```
