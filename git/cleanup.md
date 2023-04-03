---
layout: default
title: Cleanup
parent: Git and Github Actions
---

# Cleanup

## With an Action

```yaml
- name: Delete old container images
  uses: snok/container-retention-policy@v2.0.0
  with:
    image-names: "${{ env.CI_REPOSITORY_NAME }}"
    cut-off: A month ago UTC
    account-type: org
    org-name: "${{ env.GITHUB_REPOSITORY_OWNER }}"
    keep-at-least: 25
    skip-tags: latest
    untagged-only: false
    token: ${{ secrets.GITHUB_TOKEN }}
```

At the moment the GITHUB_TOKEN does not have enough permissions, so you'll need a PAT.... 
