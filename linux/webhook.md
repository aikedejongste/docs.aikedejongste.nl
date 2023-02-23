---
layout: default
title: Webook
has_children: false
parent: Linux
---

# Webook

The Debian/Ubuntu package called `webhook` is very useful. Uses systemd to run but you have to adjust the config a bit.

```bash
apt install webhook
```

The file `/lib/systemd/system/webhook.service` should contain this ExecStart command: 

```
ExecStart=/usr/bin/webhook -nopanic -hooks /etc/webhook.conf -hotreload -verbose -port 8000
```

This works but use a nicer way to do this next time. Editing this file directly is not recommended.

