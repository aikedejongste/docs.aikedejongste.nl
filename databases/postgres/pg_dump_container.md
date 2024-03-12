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
    host: <kamal managed server where this runs>
    env:
      clear:
        PGUSER: 'username'
        PGHOST: 'postgres container name or hostname'
        PGLIST: 'app_production'
        MONITORURL: 'ifconfig.me'
      secret:
        - PGPASSWORD
    directories:
      - /var/backups/:/mnt/target
    network: "private"
```

Don't forget to add `PGPASSWORD` to .env.

### Step 2: Run

```bash
kamal env push
kamal accessory boot dbbackup
```

### Step 3: change settings

Start and stop really seem to do only that. SO if you want to change the
settings, you need to remove the accessory and boot it again.

```bash
kamal accessory remove dbbackup
kamal env push
kamal accessory boot dbbackup
```




