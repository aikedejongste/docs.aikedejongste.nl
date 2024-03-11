---
layout: default
title: Postgres - pg_dump container
parent: Databases
---

# pg_dump container

## Related

- [pghoard backups](https://github.com/Aiven-Open/pghoard)

## With docker-compose

### Step 1: create .env file

All env vars should be without dashes, underscores and spaces.

```
MONITORURL='https://uptimerobot........'
PGHOST=db
PGUSER=postgres
PGPASSWORD=example
PGVERSION=16
SCHEDULE="* 4 * * *"
PGLIST=yourdbname
```

### Step 2: add to docker compose

```yaml
  dbbackup:
    image: aikedejongste/db-dumper
    container_name: dbbackup
    volumes:
      - /var/backups:/mnt/target
    environment:
      PGHOST: $PGHOST
      PGUSER: $PGUSER
      PGPASSWORD: $PGPASSWORD
      MONITORURL: $MONITOR_URL
      SCHEDULE: $SCHEDULE
      #PGLIST: app_prod
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
    networks:
      - internal
```

### Step 3: Run

```bash
docker-compose pull && docker-compose up -d && docker-compose logs -f dbbackup
```



## With Kamal

### Step 1: edit config/deploy.yaml

And add something like this:

```yaml
  dbbackup:
    image: aikedejongste/db-dumper
    env:
      clear:
        PGUSER: '....'
        PGHOST: '....'
        PGLIST: '....'
        PGLIST: '....'
        MONITORURL: 'www.uptimerobot.com/asdfasdfsadf'
      secret:
        - PGPASSWORD
    directories:
      - /var/backups/:/mnt/target
    network: "private"
```




