---
layout: default
title: Prometheus - Apache
has_children: false
parent: Monitoring
---

# Prometheus exporter for Apache 2

* [Link](https://github.com/Lusitaniae/apache_exporter)

## Requirements

Uses mod_status which is enabled by default

```bash
sudo a2enmod status
```

## Installation

```bash
apt install prometheus-exporter-apache
```

or

```yaml
  prom-apache_exporter:
    image: bitnami/apache-exporter:latest
    command: --scrape_uri="http://10.0.0.1/server-status?auto"
    ports:
      - 0.0.0.0:9117:9117
    restart: always
```

## Config in vhost

Use this to avoid ProxyPasses catching it

`ProxyPass /server-status !`

## Not in VHOST but in mods-enabled/status.conf

There is a default status location block, just add your IP addresses.

```conf
<Location "/server-status">
    SetHandler server-status
    Require ip 127.0.0.1 your_ip_address_or_network
</Location>
```

## Related:

```conf
<Location "/md-status">
  SetHandler md-status
  Require ip 127.0.0.1 1.2.3.4
</Location>
```

