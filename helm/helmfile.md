---
layout: default
title: Helmfile
parent: Helm
---

# Helmfile

## Install

* Download latest release: <https://github.com/helmfile/helmfile/releases>
* Extract and move to /usr/local/bin/helmfile

## Fixes

Error `Error: unknown command "diff" for "helm"` can be fixed with

```bash
helm plugin install https://github.com/databus23/helm-diff
```
