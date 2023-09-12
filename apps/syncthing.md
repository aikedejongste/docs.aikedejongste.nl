---
layout: default
title: Syncthing
has_children: false
parent: Self Hosted Apps
---

# Syncthing

## Installation on Ubuntu

```bash
sudo curl -o /usr/share/keyrings/syncthing-archive-keyring.gpg https://syncthing.net/release-key.gpg
echo "deb [signed-by=/usr/share/keyrings/syncthing-archive-keyring.gpg] https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list
sudo apt-get update && sudo apt-get install syncthing
```

## Start automatically for current user

```bash
sudo systemctl enable syncthing@aike.service
sudo systemctl start syncthing@aike.service
sudo systemctl status syncthing@aike.service
```
