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

