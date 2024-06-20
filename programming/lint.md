---
layout: default
title: Linting
has_children: false
parent: Programming
---

# Linting

```bash
docker run --rm -v $(pwd):/repo --workdir /repo rhysd/actionlint:latest -color
cat .github/workflows/release-and-build.yaml | docker run --rm -i rhysd/actionlint:latest -color --verbose -
docker run --rm -i hadolint/hadolint < Dockerfile
docker run --rm -v $(pwd):/data -t ghcr.io/terraform-linters/tflint
```

## Markdownlint

For commandline usage and systemwide installation:

```
npm install -g markdownlint-cli
```

Use with `markdownlint <filename>`.

## Reviewdog

TODO!

## SQL

- [sqlfluff](https://sqlfluff.com/): SQL linter and auto-formatter.

## Cytopia

- [cytopia](https://hub.docker.com/r/cytopia/ansible-lint): Ansible linting.
