---
layout: default
title: Tailscale
has_children: false
parent: Networking
---

# Tailscale

## Install

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

```bash
sudo tailscale up
```

## Use other node as exit node on Ubuntu

```bash
sudo tailscale up --exit-node=100...... --exit-node-allow-lan-access --accept-routes'
```

## Undo use exit node

```bash
sudo tailscale up --reset
```

## Config

```bash
sudo tailscale set --auto-update
```

