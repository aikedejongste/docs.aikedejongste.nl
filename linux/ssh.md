---
layout: default
title: SSH
has_children: false
parent: Linux
---

# SSH

## Use SSH to proxy SSH to other machine with config

Put this in .ssh/config 

```
Host alias-name-you-ssh-to     # use with "ssh alias-name-you-ssh-to"
  Hostname 10.10.10.10         # actual IP of destination host
  ProxyJump real.hostname.com  # actual hostname of the server you want to jump through
  User aike                    # optional username on jump host 
  Port 2222                    # optional port on jump host 
```

## Use SSH to proxy SSH to other machine cli

```bash
ssh -A -W '[10.10.10.10]:22' real.hostname
```

## Try ssh until it is ready 

```bash
until ssh 10.10.1.3; do sleep 1; done
```
or

```bash
ssh -o 'ConnectionAttempts 999' 10.10.1.3
```

