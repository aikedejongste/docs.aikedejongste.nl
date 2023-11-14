---
layout: default
title: SOP - webapp delivery
parent: SOPs
---

# Webapp Delivery

Do this before creating the pull request.

## Linters and syntax

1. Check the Dockerfile

```bash
docker run --rm -i hadolint/hadolint < Dockerfile
```

2. Supply a Github Actions workflow
That at least:

builds the image on a push
cleans up old images
3. Check your commit messages
4. Make sure you add a HealthCheck for K8s.
