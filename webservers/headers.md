---
layout: default
title: Headers
parent: Webservers and load balancing
---

# Headers

## In Apache2

```
  RequestHeader set X-Forwarded-Proto "https"
  RequestHeader set X-Forwarded-Port "443"
```

## In Traefik

```
apiVersion: helm.cattle.io/v1
kind: HelmChartConfig
metadata:
  name: traefik
  namespace: kube-system
spec:
  valuesContent: |-
    ssl:
      enabled: true
      permanentRedirect: false
    ports:
      websecure:
        tls:
          enabled: true
    logs:
      access:
        enabled: true
    additionalArguments:
      - "--entryPoints.web.proxyProtocol.insecure"
      - "--entryPoints.web.forwardedHeaders.insecure"
```

## Debug

Als je dan naar https://whoami.company.com/ gaat krijg je dit:

```
X-Forwarded-For: 84.22.102.120, 10.42.0.1
X-Forwarded-Host: whoami.company.com
X-Forwarded-Port: 443
X-Forwarded-Proto: https
X-Forwarded-Server: traefik-6f6758d9c-2tb5s
X-Real-Ip: 10.42.0.1
```

Lijkt mooi, maar is insecure, want je kunt nu alles meesturen:

```
aikedejongste@tequila ~/repos $ curl -s --header "X-Forwarded-For: 1.2.3.4" https://whoami.company.com/ | grep For
X-Forwarded-For: 1.2.3.4, 37.252.127.123, 10.42.0.1
```


## Apache Proxy Protocol Module

This is an Apache module that implements the server side of HAProxy's Proxy Protocol.

Note: as of Apache 2.4.30 this code has been merged into mod_remoteip, with the ProxyProtocol directive renamed to RemoteIPProxyProtocol.

```
<VirtualHost _default_:443>
  ServerAdmin webmaster@localhost
  RemoteIPProxyProtocol On
```

More info: [Apache - Mod RemoteIP](https://httpd.apache.org/docs/2.4/mod/mod_remoteip.html)


## Strict-Transport-Security fix:

Header always set Strict-Transport-Security “max-age=31536000; includeSubDomains”


## Referrer Policy fix:

Header always set Referrer-Policy "same-origin"



## X-Frame-Options fix:

Header always set X-Frame-Options "SAMEORIGIN"



