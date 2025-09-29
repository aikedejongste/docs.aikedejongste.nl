---
layout: default
title: Portscanning (Nmap)
parent: Linux
has_children: false
---

# Nmap

## find hosts with SSHd enabled

```bash
nmap -p 22 192.168.1.0/24 -oG - | grep 22/open
```

## ping a range

```bash
nmap -sP -R 192.168.1.1-254
```

## Scan all ports

```bash
nmap -p- 192.168.1.1
```

## Check cyphers

```bash
nmap --script ssl-enum-ciphers -p 443 <host>
```

# Naabu

## Scan range

```bash
docker run --rm -it projectdiscovery/naabu -host 192.168.1.0/24 -top-ports 1000
```
