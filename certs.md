---
layout: default
title: Certificates
has_children: false
---

# Certificates (SSL/TSL)

## Favorite vendor:

[SSLs.com](https://ssls.sjv.io/vNzVeW)

## Cert and bundle order

* /etc/ssl/certs/star.${fqdn}.crt, paste in the crt, followed by the included CA bundle, include a newline!
* /etc/ssl/private/star.${fqdn}.key, paste in the key

## Check k3s cert expiration date

```bash
openssl s_client -connect k3s.company.com:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate

## Check config at SSL Labs:

[SSL labs](https://ssllabs.com/ssltest/analyze.html)
```


