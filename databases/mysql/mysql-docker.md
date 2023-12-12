---
layout: default
title: MySQL - Docker
has_children: false
parent: Databases
---

# MySQL Docker

## Docker-compose example

```yaml
version: '3.7'

services:
  app:
    image: your-app
    container_name: app
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: example

  db:
    image: mysql
    # NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password
    # (this is just an example, not intended to be a production configuration)
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```
