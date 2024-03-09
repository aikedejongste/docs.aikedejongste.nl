---
layout: default
title: Hetzner
parent: Cloud Infrastructure
---

# Hetzner

## Authenticate

Set your API token as an env var like this: `export HCLOUD_TOKEN=kiMYCWvy`

## List floating ips

`hcloud floating-ip list`

## Floating IP with `hcloud`

```bash
hcloud floating-ip create --type ipv4 --home-location nbg1
```
