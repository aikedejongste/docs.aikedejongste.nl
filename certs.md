---
layout: default
title: Certificates
has_children: false
---

# Certificates (SSL/TSL)

## Check k3s cert expiration date

```bash
openssl s_client -connect k3s.company.com:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate
```
