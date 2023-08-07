---
layout: default
title: Graylog
parent: Logging
---

# Graylog

## Install graylog sidecar

```bash
wget https://packages.graylog2.org/repo/packages/graylog-sidecar-repository_1-2_all.deb
sudo dpkg -i graylog-sidecar-repository_1-2_all.deb
sudo apt-get update && sudo apt-get install graylog-sidecar
< edit config >
sudo graylog-sidecar -service install
sudo systemctl start graylog-sidecar
```
