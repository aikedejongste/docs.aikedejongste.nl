---
layout: default
title: Docker - Run
has_children: false
parent: Docker
---

# Docker Run Commands

## Run sleep in a container that won't start

```bash
docker run --rm --entrypoint sh ubuntu:latest -c "sleep infinity"
```
