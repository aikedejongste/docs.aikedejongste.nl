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

## Set Loki as default logging driver

Edit `/etc/docker/daemon.json`:

```yaml
{
    "debug" : true,
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "https:// <user> : <pass> @log.company.nl/loki/api/v1/push",
        "loki-batch-size": "400"
    }
}
```

## Configure with docker-compose

Add this to the container definition:

```yaml
    logging:
      driver: loki
      options:
        loki-url: "https://loki.aike.be/loki/api/v1/push"
```



