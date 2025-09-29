---
layout: default
title: PHP
has_children: false
parent: Programming
---

# PHP

## Laravel Artisan scheduler

Add this as `docker-scheduler-entrypoint`:

```bash
#!/bin/sh
set -e
CMD while true; do php artisan schedule:run --verbose --no-interaction & sleep 60; done
```

In the Dockerfile:

```bash
COPY docker-scheduler-entrypoint /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-scheduler-entrypoint
```

## Upgrade apt installed PHP version

You should use a container instead. But if you really have to:

```bash
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install php8.2 php8.2-cli php8.2-common php8.2-json php8.2-opcache php8.2-mysql php8.2-mbstring php8.2-xml php8.2-gd php8.2-curl
update-alternatives --set php /usr/bin/php8.2
a2dismod php8.1
a2enmod php8.2
systemctl restart apache2
php -v
apt remove --purge php8.1*
```
