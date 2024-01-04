---
layout: default
title: Git and Github
has_children: true
---

# Git

## Useful links

* [Git Hooks](https://www.atlassian.com/git/tutorials/git-hooks)

## Commit (add) part of a file

```bash
git add --patch <filename>

or for short:

git add -p <filename>
```

## Undo stage commit

```bash
git reset HEAD README.md

or just

git reset -- README
```

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

## Get Team ids for Oauth

```bash
export PAT= ...
export ORG= ...
curl -s -H "Authorization: token $PAT"   -H "Accept: application/vnd.github.v3+json"   https://api.github.com/orgs/$ORG/teams | jq '.[] | .id,.name'
```

## See which user a Github PAT belongs to

```bash
export PAT=..........
curl -H "Authorization: token $PAT" https://api.github.com/user
```

## See user scopes for a Github PAT

```bash
export PAT=..........
curl -s -I -H "Authorization: token $PAT" https://api.github.com/users/<YOU>
```

## Delete branches that have no remote

```bash
git fetch -p && for branch in $(git for-each-ref --format '%(refname) %(upstream:track)' refs/heads | awk '$2 == "[gone]" {sub("refs/heads/", "", $1); print $1}'); do git branch -D $branch; done
```

## Clone only a single branch

```bash
git clone https://$GLUSER:$GLPASS@gitlab.com/org/repo.git --branch staging --single-branch
```

## Download release as zipfile

```bash
curl -v -L -H "Authorization: token ghp_UaTjh0A........" https://github.com/yourcompany/yourcompany.templates/archive/refs/tags/v0.0.1.zip -o spefic-version.zip
curl -v -L -H "Authorization: token ghp_UaTjh0A........" https://github.com/yourcompany/yourcompany.templates/zipball/main -o latest-commit.zip
```
