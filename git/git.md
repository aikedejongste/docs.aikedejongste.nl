---
layout: default
title: Git and Github Actions
has_children: true
---

# Git

## Undo stage commit
```bash git reset HEAD README.md```

## Set my git config
```bash
git config --global user.email "aikedejongste@gmail.com" && git config --global user.name "Aike de Jongste"
```
## Get a zip with a release from a private repo

```bash
curl -H "Authorization: token PERSONAL_ACCESS_TOKEN_HERE" \
  http://github.com/you/reponame/zipball/main \
  -o output.zip
```
