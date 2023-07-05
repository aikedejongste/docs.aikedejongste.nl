---
layout: default
title: Cleanup
parent: Git and Github
---

# Cleanup

## With an Action

```yaml
{% raw %}
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
    token: ${{ secrets.PAT }}
{% endraw %}
```

At the moment the GITHUB_TOKEN does not have enough permissions, so you'll need a PAT....
