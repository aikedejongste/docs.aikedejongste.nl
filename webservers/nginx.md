---
layout: default
title: Nginx
parent: Webservers and load balancing
---

# Nginx

## Security.txt

```
    location /.well-known/security.txt {
        add_header Content-Type 'text/plain';
        add_header Cache-Control 'no-cache, no-store, must-revalidate';
        add_header Pragma 'no-cache';
        add_header Expires '0';
        add_header Vary '*';
        return 200 "Contact: mailto:security@ you .nl";
  }
```

## Simple hello world config

```
worker_processes 1;
error_log stderr notice;
events {
    worker_connections 1024;
}

http {
    variables_hash_max_size 1024;
    access_log off;
    real_ip_header X-Real-IP;
    charset utf-8;

    server {
        listen 80;

        location / {
            return 200 "hello web1";
        }

        location /static/ {
            alias static/;
        }
    }
}
```
