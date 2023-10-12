---
layout: default
title: Bitbucket pipelines
parent: Git and Github
---

# Bitbucket pipelines

## Simple Docker build and push

```yaml
image: atlassian/default-image:3

pipelines:
  branches:
    master:
      - step:
          name: Build and Push
          script:
            # TODO: check if variables are set
            - echo ${ACR_PASSWORD1} | docker login gapphonline.azurecr.io --username "$ACR_USERNAME" --password-stdin
            - IMAGE=gapphonline.azurecr.io/gapphonline
            - TAG="${BITBUCKET_BUILD_NUMBER}"
            - docker build . --file Dockerfile --tag "${IMAGE}:${TAG}"
            - docker push "${IMAGE}:${TAG}"
            - docker tag "${IMAGE}:${TAG}" "${IMAGE}:latest"
            - docker push "${IMAGE}:latest"
          services:
            - docker
```
