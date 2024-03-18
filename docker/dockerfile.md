---
layout: default
title: Dockerfile
has_children: false
parent: Docker
---

# Dockerfile

## SSH in Dockerfile (ugly!)

```bash
RUN mkdir -p -m 0600 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
RUN ssh-agent sh -c 'echo "$SSH_KEY" | ssh-add - ; npm install --force'
```
