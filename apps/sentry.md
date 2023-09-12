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

## Kafka consumer issues

Will probably result in notifications not being triggered and alerts not being created. Check one of the consumer container logs to see if there are errors.

Troubleshooting guide: [link](https://github.com/getsentry/develop/blob/master/src/docs/self-hosted/troubleshooting.mdx)

Reset every group to latest:

```bash
docker-compose run --rm kafka kafka-consumer-groups --bootstrap-server kafka:9092 --all-groups --all-topics --reset-offsets --to-latest --execute
```

Stop all consumers:

```bash
stop-snuba.sh
docker-compose stop subscription-consumer-transactions
docker-compose stop subscription-consumer-events
docker-compose stop post-process-forwarder-errors
docker-compose stop post-process-forwarder-transactions
docker-compose stop worker
docker-compose stop snuba-subscription-consumer-transactions
docker-compose stop snuba-outcomes-consumer
docker-compose stop snuba-sessions-consumer
docker-compose stop snuba-transactions-consumer
docker-compose stop snuba-api
docker-compose stop snuba-transactions-cleanup
docker-compose stop snuba-cleanup
docker-compose stop snuba-subscription-consumer-events
docker-compose stop snuba-replacer
docker-compose stop snuba-consumer
docker-compose stop symbolicator-cleanup
docker-compose stop ingest-consumer
```

## Prometheus exporter

The first one only exports 1 metric.

```yaml
  sentry-exporter1:
    container_name: "sentry-exporter1"
    image: ghcr.io/ujamii/prometheus-sentry-exporter
    environment:
      AUTH_TOKEN:
      SENTRY_HOST: sentry.you.app
  sentry-exporter2:
    container_name: "sentry-exporter2"
    image: italux/sentry-prometheus-exporter
    environment:
      SENTRY_BASE_URL: "https://sentry.you.app/api/0/"
      SENTRY_AUTH_TOKEN: "...."
      SENTRY_EXPORTER_ORG: "you"
      SENTRY_EXPORTER_PROJECTS: "you"
```

