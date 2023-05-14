---
layout: default
title: Github Actions
has_children: true
---

# Git

## Usefull links:

* [Git Hooks](https://www.atlassian.com/git/tutorials/git-hooks)

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

## Set Git hooks directory to .githooks

```bash git config core.hooksPath .githooks```


## Clone or push over https with Fine Grained PAT

```bash
export GITHUB_USER=your-username
export GITHUB_TOKEN=github_pat_1234..........
export GITHUB_REPO=org/repo
git clone https://${GITHUB_USER}:${GITHUB_TOKEN}@github.com/${GITHUB_REPO}

OR

git remote add origin https://${GITHUB_USER}:${GITHUB_TOKEN}@github.com/${GITHUB_REPO}
git push --set-upstream origin master
```
