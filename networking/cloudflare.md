---
layout: default
title: Allow from CloudFlare
parent: Networking
---

# Allow traffic from CloudFlare with UFW

```bash
#! /bin/bash

apt install ufw -y
for i in `curl https://www.cloudflare.com/ips-v4`; do ufw allow from $i to any port http; done
for i in `curl https://www.cloudflare.com/ips-v4`; do ufw allow from $i to any port https; done
for i in `curl https://www.cloudflare.com/ips-v6`; do ufw allow from $i to any port http; done
for i in `curl https://www.cloudflare.com/ips-v6`; do ufw allow from $i to any port https; done
```

# Allow traffic from CloudFlare with Iptables

```bash
#!/bin/bash

# https://www.cloudflare.com/ips
# https://support.cloudflare.com/hc/en-us/articles/200169166-How-do-I-whitelist-CloudFlare-s-IP-addresses-in-iptables-

for i in `curl https://www.cloudflare.com/ips-v4`; do iptables -I INPUT -p tcp -m multiport --dports http,https -s $i -j ACCEPT; done
for i in `curl https://www.cloudflare.com/ips-v6`; do ip6tables -I INPUT -p tcp -m multiport --dports http,https -s $i -j ACCEPT; done

iptables -A INPUT -p tcp -m multiport --dports http,https -j DROP
ip6tables -A INPUT -p tcp -m multiport --dports http,https -j DROP
```
