---
layout: default
title: Publishing Charts
parent: Helm
---

# Publishing Chatrs

## GHCR

```bash
helm create <chart-name>
helm package <chart-name>
echo $GHCR_PAT | docker login ghcr.io -u <GITHUB-USERNAME> --password-stdin
export CHART_VERSION=$(grep 'version:' ./path/to/Chart.yaml | tail -n1 | awk '{ print $2 }')
helm push <chart-name>-${CHART_VERSION}.tgz oci://ghcr.io/<GITHUB-USERNAME>
```
