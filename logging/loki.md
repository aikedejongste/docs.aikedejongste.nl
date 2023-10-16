---
layout: default
title: Loki docker driver
parent: Logging
---

# Loki Docker driver

## Install

```bash
docker plugin install grafana/loki-docker-driver:2.9.1 --alias loki --grant-all-permissions
```

## Configure with docker-compose

Add this to the container definition:

```yaml
    logging:
      driver: loki
      options:
        loki-url: "https://loki.aike.be/loki/api/v1/push"
```



