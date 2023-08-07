---
layout: default
title: Github Actions Secrets
parent: Git and Github
---

# Github Actions Secrets

## Check if secret or env var is set

```yaml

- name: Check if an env var is set
  run: |
    if [ -z "${{ env.DLD_TEMPLATES_RELEASE }}" ]; then
      echo "ENV DLD_TEMPLATES_RELEASE is NOT set"
      exit 1
    else
      echo "ENV DLD_TEMPLATES_RELEASE is set"
    fi

- name: Check if a secret is set
  run: |
    if [ -z "${{ secrets.DLD_TEMPLATES_RELEASE }}" ]; then
      echo "DLD_TEMPLATES_RELEASE is NOT set"
      exit 1
    else
      echo "DLD_TEMPLATES_RELEASE is set"
    fi

- name: Check if GITHUB secret is set (always true)
  run: |
    if [ -z "${{ secrets.GITHUB_TOKEN }}" ]; then
      echo "GITHUB_TOKEN is NOT set"
      exit 1
    else
      echo "GITHUB_TOKEN is set"
    fi
```
