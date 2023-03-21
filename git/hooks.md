---
layout: default
title: Hooks
parent: Git and Github Actions
---

# Hooks

## Pre-commit

### Message length check

```bash
#!/usr/bin/env bash

# Hook to make sure that no commit message line exceeds 50 characters

while read line; do
    # Skip comments
    if [ "${line:0:1}" == "#" ]; then
        continue
    fi
    if [ ${#line} -ge 50 ]; then
        echo "Commit messages are limited to 50 characters."
        echo "The following commit message has ${#line} characters."
        echo "${line}"
        exit 1
    fi
done < "${1}"

exit 0
```
