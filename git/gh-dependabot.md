---
layout: default
title: Github - Dependabot
parent: Git and Github
---

# Dependabot

## .github/dependabot.yml

```yaml
version: 2
updates:
  - package-ecosystem: "bundler"
    schedule:
      interval: "weekly"
      day: "monday"
      time: "08:00"
#    open-pull-requests-limit: 0 # Prevents opening PRs for version updates
#    ignore: # Ignore all versions, effectively only allowing security updates
#      - dependency-name: "*"
#        versions: ["*"]
```
