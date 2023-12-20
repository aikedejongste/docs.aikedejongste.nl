---
layout: default
title: Redis
has_children: false
parent: Self Hosted Apps
---

# Redis

## Docker compose

```yaml
version: "3"

services:
  redis:
    container_name: redis
    image: redis:7
    volumes:
      - './redis_data:/data'
    restart: unless-stopped
```
