---
layout: default
title: Powershell
has_children: false
parent: Programming
---

# Powershell

## Use WSL in PS

Check which subsystems are available:
```bash
wsl --list
Windows Subsystem for Linux Distributions:
docker-desktop-data (Default)
Ubuntu
docker-desktop
```
That docker-desktop-data default seemed to be the problem.

Change the default

```bash
> wsl --setdefault Ubuntu
```



