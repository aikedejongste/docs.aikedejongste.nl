---
layout: default
title: Sentry
has_children: false
parent: Self Hosted Apps
---

# Sentry


## Cleanup 

```bash
/usr/bin/docker-compose --file /path/to/docker-compose.yml exec worker sentry cleanup --days 30
```

And maybe do a VACUUM in Postgres as well:

```bash
docker exec -it sentry-self-hosted_postgres_1 bash 
or
docker-compose exec postgres bash

psql -U postgres

VACUUM FULL;
```

