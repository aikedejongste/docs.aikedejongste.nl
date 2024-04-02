---
layout: default
title: Simple container build
parent: Git and Github
---

# Links

- [env-vars-action](https://github.com/FranzDiebold/github-env-vars-action)

# Very simple container build

```yaml
name: Build Docs container

on:
  workflow_dispatch:
  push:
    branches:
    - main

env:
  REGISTRY: ghcr.io
  IMAGE: "${{ github.repository }}"

permissions: write-all

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: GitHub Environment Variables Action
      uses: FranzDiebold/github-env-vars-action@v2

    - name: Log in to the Container registry
      uses: docker/login-action@v3
      with:
        registry: "${{ env.REGISTRY }}"
        username: "${{ github.actor }}"
        password: "${{ secrets.GITHUB_TOKEN }}"

    - name: Build the Docker image
      run: docker build . --label "org.opencontainers.image.source=https://github.com/${{ env.IMAGE }}" --tag "ghcr.io/${{ env.IMAGE }}:latest" --file Dockerfile

    - name: Docker push with latest tag
      run: docker push "ghcr.io/${{ env.IMAGE }}:latest"
```

# Simple container build

```yaml
{% raw %}

name: Build and publish container image

on:
  push:
    branches: [ "main", "master"]
  pull_request:
    branches: [ "main", "master" ]
  workflow_dispatch:

env:
  REGISTRY: ghcr.io
  IMAGE: ${{ github.repository }}

jobs:
  build-and-push-image:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Log in to the Container registry
        uses: docker/login-action@v2.2.0
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@v4.6.0
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v4.1.1
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
{% endraw %}

```
