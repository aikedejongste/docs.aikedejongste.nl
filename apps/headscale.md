---
layout: default
title: Headscale
has_children: false
parent: Self Hosted Apps
---

# Headscale


## Add node on the server

You need to exec into the container to create API keys for the webinterface. And also to add 'users'.

For example:
```bash
headscale --namespace company-name preauthkeys create --reusable --expiration 96h
```

## On the lient

```bash
sudo tailscale up --login-server https://hs.company-name.com --auth-key 726853 <kniiiiiiiip> 74e300e
```

## List nodes

You can get an overview of the current nodes in the webinterface and in the container:

```bash
root@headscale1:~# docker exec -it headscale /bin/sh
/ # headscale node list
ID | Hostname  | Name             | NodeKey | Namespace      | IP addresses | Online | Expired
1  | node1     | aike-work-laptop | [abcde] | company-name   | 100.64.0.1   | online | no     
2  | node2     | node2            | [edcba] | company-name   | 100.64.0.2   | online | no     
```


