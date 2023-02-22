---
layout: default
title: Minio
has_children: false
parent: Self Hosted Apps
---

# Minio

* Don't forget to enable websockets for the web console.

## Apache2 ProxyPass config example

```
RewriteEngine On

RewriteCond %{HTTP:UPGRADE} ^WebSocket$ [NC]
RewriteCond %{HTTP:CONNECTION} Upgrade$ [NC]
RewriteRule /ws/(.*) ws://10.1.1.1:9001/ws/$1 [P]

<Location />
   <RequireAll>
     <RequireAny>
       Require ip 1.2.3.4
     </RequireAny>
   </RequireAll>
</Location>

ProxyPreserveHost On
ProxyPass / http://10.1.1.1:9001/
ProxyPassReverse / http://10.1.1.1:9001/
```

## Docker-compose example

```yaml
version: '3'

services:
  minio:
    container_name: minio
    image: minio/minio
    restart: unless-stopped
    security_opt:
      - seccomp:unconfined
    ports:
      - "0.0.0.0:9000:9000"
      - "0.0.0.0:9001:9001"
    volumes:
      - /opt/minio/data:/data
    environment:
      MINIO_ROOT_USER: user
      MINIO_ROOT_PASSWORD: password
    command: server --console-address ":9001" /data

```
