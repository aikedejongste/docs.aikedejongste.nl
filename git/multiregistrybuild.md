---
layout: default
title: Multiple registry build
parent: Git and Github
---

# Multiple registry build

```yaml
{% raw %}
name: Build and publish

on:
  push:
    branches: [ "main", "master"]
  pull_request:
    branches: [ "main", "master" ]
  workflow_dispatch:


env:
  REGISTRY1: ghcr.io
  REGISTRY2: registry.digitalocean.com
  IMAGE1: ${{ github.repository }}-portal
  IMAGE2: company/appname

jobs:
  build-and-push-image:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Log in to the GITHUB Container registry
        uses: docker/login-action@f054a8b539a109f9f41c372932f1ae047eff08c9
        with:
          registry: ${{ env.REGISTRY1 }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Log in to the DIGITAL OCEAN Container registry
        uses: docker/login-action@f054a8b539a109f9f41c372932f1ae047eff08c9
        with:
          registry: ${{ env.REGISTRY2 }}
          username: ${{ secrets.DOREGISTRYTOKEN }}
          password: ${{ secrets.DOREGISTRYTOKEN }}

      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@master
        with:
          images: |
            ${{ env.REGISTRY1 }}/${{ env.IMAGE1 }}
            ${{ env.REGISTRY2 }}/${{ env.IMAGE2 }}

      - name: Build and push Docker image
        uses: docker/build-push-action@ad44023a93711e3deb337508980b4b5e9bcdc5dc
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}

{% endraw %}

```
