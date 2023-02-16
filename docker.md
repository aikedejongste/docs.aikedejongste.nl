---
layout: default
title: Docker
#nav_order: 3
has_children: false
---

# Docker

## Remove all containers from a host:
```bash
docker rm -f $(docker ps -qa)
```
