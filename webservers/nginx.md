---
layout: default
title: Nginx
parent: Webservers
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
