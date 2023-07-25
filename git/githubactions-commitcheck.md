---
layout: default
title: Github Actions Check Commits
parent: Git and Github
---

# Github Actions Check Commits


## Check for conventional commits and project number

```yaml
name: CommitChecks

on:
  workflow_dispatch:
  workflow_call:
  pull_request:
    branches: [master, main]
    types: [opened, edited, reopened, synchronize]
  pull_request_target:
    types: [opened, edited, reopened, synchronize]

jobs:
  check-for-cc:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      # interesting alternative: https://github.com/cocogitto/cocogitto
      - name: Conventional commit checker
        uses: webiny/action-conventional-commits@v1.1.0
        if: ${{ github.event_name != 'workflow_dispatch' }}

      - name: Check Card# reference
        uses: gsactions/commit-message-checker@v2
        with:
          pattern: 'FF-\d\d\d'
          flags: 'gm'
          error: 'Your commit message has to end with a project number like FF-173".'
          excludeDescription: 'true' # optional: this excludes the description body of a pull request
          excludeTitle: 'true' # optional: this excludes the title of a pull request
          checkAllCommitMessages: 'true' # optional: this checks all commits associated with a pull request
          accessToken: ${{ secrets.GITHUB_TOKEN }} # github access token is only required if checkAllCommitMessages is true
        if: ${{ github.event_name != 'workflow_dispatch' }}

```
