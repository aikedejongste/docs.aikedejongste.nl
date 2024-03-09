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

