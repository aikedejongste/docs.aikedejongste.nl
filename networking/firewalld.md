---
layout: default
title: Firewalld
parent: Networking
---

# Firewalld

## Install on Ubuntu
```bash
apt install firewalld 
```

## Show status
```bash
firewall-cmd --state
```

## Show open ports
```bash
firewall-cmd --list-ports
```

### Show configuration
```bash
firewall-cmd --list-all
```

## Allow a named service or a port
```bash 
firewall-cmd --permanent --add-service=nfs3

OR

firewall-cmd --add-port=514/udp --permanent
```

## Access from specific source ip (create a zone)

```bash
firewall-cmd --new-zone=mariadb-access --permanent
firewall-cmd --reload
firewall-cmd --get-zones
firewall-cmd --zone=mariadb-access --add-source=10.24.96.5/24 --permanent
firewall-cmd --zone=mariadb-access --add-port=3306/tcp  --permanent
firewall-cmd --reload
```


## Port forwarding
```bash
# Enable masquerading
firewall-cmd --add-masquerade --permanent

# Port forward to same port on a different server (local:22 > 192.168.2.10:22)
firewall-cmd --add-forward-port=port=22:proto=tcp:toaddr=192.168.2.10 --permanent

# Port forward to different port on a different server (local:7071 > 10.50.142.37:9071)
firewall-cmd --add-forward-port=port=7071:proto=tcp:toport=9071:toaddr=10.50.142.37 --permanent
```
