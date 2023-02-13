---
layout: default
title: IPv6
#nav_order: 3
has_children: false
---

# IPv6

## Disable IPv6 the evil way:

```
rm -f /etc/sysctl.conf && echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf && echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf && echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf && sysctl -p
```

