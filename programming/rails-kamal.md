---
layout: default
title: Rails - Kamal
has_children: false
parent: Programming
---

# Deploy Rails with Kamal

## Links

- [Traefik Dashboard](https://dev.to/tannakartikey/how-to-enable-traefik-dashboard-with-mrsk-48ef?comments_sort=latest)
- [Zot on Kamal](apps/zot.md)

## Docker credentials

If you get a Docker registry config with username and password in 1 you can separate them like this:

```bash
cat docker-config.json | jq -r '.auths | to_entries | .[0].key'
cat docker-config.json | jq -r '.auths | to_entries | .[0].value.auth' | base64 -d | cut -d ":" -f 1
cat docker-config.json | jq -r '.auths | to_entries | .[0].value.auth' | base64 -d | cut -d ":" -f 2
```


## Init Kamal

- bundle add kamal
- kamal init

Database.yaml
password: <%= ENV["POSTGRES_PASSWORD"] %>
host: <%= ENV["DB_HOST"] %>

10. assume ssl! not force


mkdir -p /opt/letsencrypt && touch /opt/letsencrypt/acme.json && chmod 600 /opt/letsencrypt/acme.json

docker network create -d bridge private


.env has priority over key files in config/credentials

but you need a master.key file?



## Publish Traefik dashboard

```yaml
traefik:
  options:
    publish:
      - "443:443"
    volume:
      - "/opt/letsencrypt/acme.json:/letsencrypt/acme.json"
    network: "private"
  args:
    accesslog: true
    api.dashboard: true
    accesslog.format: json
    entryPoints.web.address: ":80"
    entryPoints.websecure.address: ":443"
    certificatesResolvers.letsencrypt.acme.email: "noc@company.com"
    certificatesResolvers.letsencrypt.acme.storage: "/letsencrypt/acme.json"
    certificatesResolvers.letsencrypt.acme.httpchallenge: true
    certificatesResolvers.letsencrypt.acme.httpchallenge.entrypoint: web
  labels:
    traefik.enable: true
    traefik.http.routers.dashboard.rule: Host(`traefik.company.com`)
    traefik.http.routers.dashboard.service: api@internal
    traefik.http.routers.dashboard.middlewares: auth
    traefik.http.routers.dashboard.tls: true
    traefik.http.routers.dashboard.entrypoints: websecure
    traefik.http.routers.dashboard.tls.certresolver: letsencrypt
    traefik.http.middlewares.auth.basicauth.users: user:$apr1$oWHu5uDx$w6nxBoxlloZrScp9d2C2I1
```
