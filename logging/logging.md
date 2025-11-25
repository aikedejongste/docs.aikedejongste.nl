---
layout: default
title: Logging
#nav_order: 3
has_children: true
---

# Logging

## Log levels explanation

![Log levels](/images/levels.png)

## Quick log to Loki

```bash
docker run -v /var/log:/var/log -e LOG_PATH="/var/log/syslog.log" -e LOKI_URL="https://loki.company.com/loki/api/v1/push" grafana/fluent-bit-plugin-loki:latest
```
