---
layout: default
title: Digital Ocean
parent: Cloud Infrastructure
---

# Digital Ocean

## Links

Firewall: [here](https://docs.aikedejongste.nl/linux-utils/webhook.html#firewall-on-digital-ocean)

## doctl installation

```bash
pip install lastversion && wget https://github.com/digitalocean/doctl/releases/download/v`lastversion doctl`/doctl-`lastversion doctl`-linux-amd64.tar.gz -O /tmp/doctl.tgz
tar -zxvf /tmp/doctl.tgz && sudo mv /tmp/doctl /usr/local/bin/ && sudo chmod +x /usr/local/bin/doctl
```

## doctl auth

```
doctl auth init
```

Go to [https://cloud.digitalocean.com/account/api/tokens](https://cloud.digitalocean.com/account/api/tokens) and create a token.
