---
layout: default
title: Syntax and Linting
parent: Ansible
---

# Syntax and Linting

## .ansible-lint

```yaml
skip_list:
  - yaml
  - latest[git]
```

## .ansible-lint-ignore

```yaml
# This file contains ignores rule violations for ansible-lint
roles/whoami/tasks/main.yaml yaml[indentation]
```
