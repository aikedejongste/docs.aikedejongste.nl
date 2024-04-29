---
layout: default
title: Redis
parent: Databases
---

# Digital Ocean managed redis

## Connect

```
redli --tls -h you.db.ondigitalocean.com -a passssss -p 25061
```

## Limit memory usage in Sentry
```
docker exec -i sentry-self-hosted_redis_1 redis-cli info
docker exec -i sentry-self-hosted_redis_1 redis-cli CONFIG SET maxmemory 4G
docker exec -i sentry-self-hosted_redis_1 redis-cli CONFIG SET maxmemory-policy volatile-ttl
```

## Fix Redis won't start with systemd on CentOS 7

In /etc/redis.conf

```
#supervised no
supervised auto
#daemonize no
daemonize yes
```

And then change the systemd service file `systemctl edit redis`:

```
[Service]
Type=forking
```

Probably not necessary, but I also installed `systemd-devel`:

```bash
yum install systemd-devel
```

