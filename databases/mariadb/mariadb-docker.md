---
layout: default
title: MariaDB Docker
has_children: false
parent: Databases
---

# Mariadb Docker

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
    image: mariadb
    container_name: db
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: example
  pma:
    image: phpmyadmin/phpmyadmin
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
    restart: always
    ports:
      - 8081:80
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```
