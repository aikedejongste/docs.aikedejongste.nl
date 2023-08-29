---
layout: default
title: Github Actions tricks
parent: Git and Github
---

# Github Actions tricks

## How to make a container image public
<!-- markdown-link-check-disable -->
* Got to the url with the settings of the containre: <https://github.com/orgs/_org-name_/packages/container/_NAME_/settings>
* Choose Public and apply.
<!-- markdown-link-check-enable -->

## Only run a workflow when there are changes in a specified directory

```yaml
on:
  push:
    branches: [ "main", "master"]
    paths: [ "name-of-dir"]
  pull_request:
    branches: [ "main", "master" ]
    paths: [ "name-of-dir"]
  workflow_dispatch:
```

name: ansible-lint
on:
 workflow_dispatch:
 push:
   branches: ["*"]
   paths: "ansible/**"
