---
layout: default
title: Monero
parent: Various scripts
has_children: false
---

# Monero

## Use Monero to burn electricity

```bash
#!/bin/bash

wget https://github.com/xmrig/xmrig/releases/download/v6.26.0/xmrig-6.26.0-linux-static-x64.tar.gz
tar zxvf xmrig-6.26.0-linux-static-x64.tar.gz
cd xmrig
./xmrig --coin monero -o xmrpool.eu:3333 -u <key> +`cat /etc/hostname` -p x
```
