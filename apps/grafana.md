---
layout: default
title: Grafana
has_children: false
parent: Self Hosted Apps
---

# Grafana Docker

## Reset admin password

```bash
docker exec -it grafana bash
grafana cli admin reset-admin-password 1234567890
```

## Run in Docker

With a docker-compose.yaml:

```yaml
version: "3"

networks:
  internal:

services:
  grafana:
    container_name: grafana
    image: grafana/grafana:latest
    restart: always
    user: "472"
    ports:
      - "3000:3000"
    volumes:
      - /opt/grafana-config/grafana.ini:/etc/grafana/grafana.ini
      - /opt/grafana-data:/var/lib/grafana
    networks:
      - internal
```

## Permissions

```bash
chown -R 472:472 /opt/grafana*
```

## Configuration

```bash
docker cp grafana:/etc/grafana/grafana.ini grafana-config/grafana.ini
```

Set domain:

```ini
domain = grafana.aike.be
enforce_domain = false
root_url = %(protocol)s://%(domain)s:%(http_port)s/
```

## OAuth / OIDC troubleshooting

### redirect_uri
If you get an error about the redirect url when redirect from Grafana to Keycloak it is probably
the root_url that is not set in Grafana.

### newtransportwithcode
`login.OAuthLogin (NewTransportWithCode) after OAuth login` -> it cannot connect to the authentication
server, so maybe a DNS or a firewall problem?


