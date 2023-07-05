---
layout: default
title: Github Actions tricks
parent: Git and Github
---

# Github Actions tricks


## How to make a container image public:

* Got to the url with the settings of the containre: https://github.com/orgs/<org-name>/packages/container/<name>/settings
* Choose Public and apply.

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
