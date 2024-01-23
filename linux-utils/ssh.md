---
layout: default
title: SSH
has_children: false
parent: Linux
---

# SSH

## Links

- [SSHSnake](https://joshua.hu/ssh-snake-ssh-network-traversal-discover-ssh-private-keys-network-graph)

## Add public key to server from Github

```bash
mkdir -p -m 700 ~/.ssh; echo "$(curl -s https://github.com/aikedejongste.keys) # Aike" >> ~/.ssh/authorized_keys; chmod 600 ~/.ssh/authorized_keys
```

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

## Generate key

```bash
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"
```

## AutoSSH

Make a firewall host reachable over reverse SSH tunnel with AutoSSH:

```bash
autossh -nNT -R 2222:localhost:22 user@remote.box
```

so you can ssh to remote.box:2222.

## Ansible add ssh fingerprint to known hosts

```yaml
- name: update known hosts file
  shell: ssh-keyscan -H "{{ item }}" >> ~/.ssh/known_hosts
  loop:
    - 10.10.1.2
```
