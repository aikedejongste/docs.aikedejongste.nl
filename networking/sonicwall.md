---
layout: default
title: SonicWall VPN
parent: Networking
---

# SonicWall Connect Tunnel VPN on Debian

## Install dependencies

```bash
apt install ca-certificates-java java-common default-jre
```

## Install the client

```bash
./install.sh
```

## Connect

```bash
startct --mode console --name your-company --server vpn.company.eu --username aike
```
