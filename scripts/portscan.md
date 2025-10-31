---
layout: default
title: Simple Portscan
has_children: false
parent: Scripts
---

## Simple portscan

```
#!/bin/bash

echo "Scan started at: $(date)"

for port in {1..65535}; do
  timeout 1 bash -c "echo >/dev/tcp/$1/$port" && echo "Port $port is open"
      #|| echo "Port $port is closed"
done

echo "Scan completed at: $(date)"
```
