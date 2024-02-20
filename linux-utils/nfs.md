---
layout: default
title: NFS
has_children: false
parent: Linux
---

# NFS

## NFS Client

### Install dependencies

```bash
apt install nfs-common
```

### Check firewall on server from client

```bash
rpcinfo -p 10.0.0.1
```

### Check available mounts on server from client

```bash
showmount -e 10.0.0.1
```

### Mount with fstab

Use \040 (octal) for mounts with spaces in the name

```
10.0.0.1:/mnt/data         /mnt/here           nfs defaults  0 0
```

### Mount in docker-composeo

```yaml
volumes:
  private:
    driver_opts:
      type: "nfs"
      o: "addr=10.1.1.1,ro"
      device: ":/mnt/data"
```

## NFS Server

## Install packages

```bash
apt install nfs-kernel-server
```

### Show ports NFS is using

Run this on the server:

```bash
rpcinfo -p | awk '{print $3" "$4}' | sort -k2n | uniq
```

### Force NFS to use a fixed port

This changed with Ubuntu 22. It used to be configured in /etc/defaults/ now edit `/etc/nfs.conf`

```
[mountd]
manage-gids=y
port=2000
```

On old Ubuntu versions:

This only works on old Ubuntu versions: `RPCMOUNTDOPTS="--manage-gids -p 2000"`

## Export directory

Use quotes for dirs with spaces, escaping with \ does not seem to work.

```bash
/mnt/data 10.0.0.*(rw,sync,no_root_squash,no_subtree_check)
```

Available options explained:

* no_all_squash / all_squash :

a) no_all_squash : does not change the mapping of remote users.

b) all_squash : to squash all remote users including root.

* root_squash / no_root_squash :

a) root_squash : prevent root users connected remotely from having root access. Effectively squashing remote root privileges.

b) no_root_squash : disable root squashing.

## Give new files uid/guid 33 (www-data)

So even if root or another user creates a file, the new owner will be 33:33.

```
"/opt/whatever" 10.1.2.3(rw,sync,no_subtree_check,all_squash,anonuid=33,anongid=33)
```

## Configure firewall

### Manually

```bash
sudo iptables -A INPUT -d 10.0.0.0/24 -s 10.0.0.0/24 -p tcp -m multiport --ports 111,2000,2001,2049 -j ACCEPT
sudo iptables -A INPUT -d 10.0.0.0/24 -s 10.0.0.0/24 -p udp -m multiport --ports 111,2000,2001,2049 -j ACCEPT
```

### With UFW (only port 2049)

```bash
ufw allow from 10.0.0.0/24 to any port nfs
more for the other ports?
```

### With firewall-cmd

```bash
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=rpcbind
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
```

## Troubleshooting

- nfsiostat 5
