---
layout: default
title: GitLAB
parent: Git and Github
---

# GitLAB

## Gitlab runner with Helm

```bash
export RT=123456789
export NS=gitlabrunner

helm repo add gitlab https://charts.gitlab.io

helm install gitlab-runner -n $NS --set gitlabUrl=https://gitlab.com,runnerRegistrationToken=$RT gitlab/gitlab-runner
```

## Clone with deploy token

```bash
export GLUSER=username
export GLPASS=tokentokentoken

git clone https://$GLUSER:GLPASS@gitlab.com/org/project/repo.git
```

## Simple buildx Docker build

```yaml
docker-build-main:
  image: docker:latest
  stage: build
  services:
    - docker:dind
  before_script:
    - docker info
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    - docker buildx create --use --name=buildkit-builder
  script: |
    docker buildx build \
        --tag "$CI_REGISTRY_IMAGE" \
        --cache-from type=registry,ref=$CI_REGISTRY_IMAGE:cache \
        --cache-to type=registry,ref=$CI_REGISTRY_IMAGE:cache \
        --push \
        .
  only:
    - main
```
