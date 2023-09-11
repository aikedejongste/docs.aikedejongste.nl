---
layout: default
title: Github Actions - 403
parent: Git and Github
---

# Github Actions Check url unreachable

## Workflow

Yes, I'm checking if an url is UNreachable

```yaml
name: Check if URLs are unreachable

# Using Github Actions to run a cron job to check if the URLs are unreachable.
# This is not what Actions is meant for but this is so incredibly simple that it should be okay.

on:
  workflow_dispatch:
  schedule:
    - cron: "0 18 * * *"

env:
  URL_LIST: "https://www.aikedejongste.nl

jobs:
  check-urls:
    runs-on: ubuntu-latest
    steps:
      - name: Check URLs
        run: |
          IFS=',' read -ra urls <<< "$URL_LIST"

          for url in "${urls[@]}"; do
            status_code=$(curl --write-out '%{http_code}' --silent --output /dev/null "$url")

            if [[ "$status_code" == "000" ]]; then
              echo "Error: URL $url is unreachable. This is good."
            elif [[ "$status_code" == "403" ]]; then
              echo "Error: URL $url returned a 403 Forbidden status"
            else
              echo "URL $url returned status code $status_code. This is probably bad."
              exit 1
            fi
          done
```
