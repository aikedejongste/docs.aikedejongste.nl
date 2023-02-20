---
layout: default
title: Networking
has_children: true
---

# Networking

## Disable cloud-init after changing network config

```bash
touch /etc/cloud/cloud-init.disabled
```


## Open Ngrok tunnel

```bash
ngrok http --region=eu --hostname=aike.eu.ngrok.io 4567
```

