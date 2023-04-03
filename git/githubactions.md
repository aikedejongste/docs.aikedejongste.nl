---
layout: default
title: Github Actions tricks
parent: Git and Github Actions
---

# Github Actions tricks

## Only run a workflow when there are changes in a specified directory

```yaml
on:
  push:
    branches: [ "main", "master"]
    path: [ "name-of-dir"]
  pull_request:
    branches: [ "main", "master" ]
    path: [ "name-of-dir"]
  workflow_dispatch:
```
