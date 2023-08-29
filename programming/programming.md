---
layout: default
title: Programming
has_children: true
---

# Linting

```bash
docker run --rm -v $(pwd):/repo --workdir /repo rhysd/actionlint:latest -color
cat .github/workflows/release-and-build.yaml | docker run --rm -i rhysd/actionlint:latest -color --verbose -
docker run --rm -i hadolint/hadolint < Dockerfile
docker run --rm -v $(pwd):/data -t ghcr.io/terraform-linters/tflint
```

reviewdog!
