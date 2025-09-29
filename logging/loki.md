---
layout: default
title: Loki
parent: Logging
---

# Loki

Grafana Loki needs to store two different types of data: **chunks** and **indexes**.

## Filesystem

The filesystem object store is the easiest to get started with Grafana Loki but there are some pros/cons to this approach.

Very simply it stores all the objects (chunks) in the specified directory:

```yaml
storage_config:
  filesystem:
    directory: /tmp/loki/
```

## BoltDB

BoltDB Shipper lets you run Grafana Loki without any dependency on NoSQL stores for storing index.
It locally stores the index in BoltDB files instead and keeps shipping those files to a shared
object store i.e the same object store which is being used for storing chunks.

It is a requirement to have index period set to 24h for usage of boltdb-shipper.

Compactor is a BoltDB Shipper specific service that reduces the index size by deduping the index and
merging all the files to a single file per table. We recommend running a Compactor since a single
Ingester creates 96 files per day which include a lot of duplicate index entries and querying
multiple files per table adds up the overall query latency.

Example config:

```
yaml
schema_config:
  configs:
    - from: 2018-04-15
      store: boltdb-shipper
      object_store: gcs
      schema: v11
      index:
        prefix: loki_index_
        period: 24h

compactor:
  working_directory: /loki/compactor
  shared_store: gcs

storage_config:
  gcs:
    bucket_name: GCS_BUCKET_NAME

  boltdb_shipper:
    active_index_directory: /loki/index
    shared_store: gcs
    cache_location: /loki/boltdb-cache
```

## Table manager and retention

The object storages - like Amazon S3 and Google Cloud Storage - supported by Loki to store chunks,
are not managed by the Table Manager, and a custom bucket policy should be set to delete old data.

Looks like you don't want to use the table manager.

## Retention

See the logs `docker logs -f loki_loki_1 -n 5000 2>&1 | grep compactor`

Useful info: <https://github.com/grafana/loki/issues/6300#issuecomment-1431887039>

Retention through the [Table
Manager](https://grafana.com/docs/loki/latest/operations/storage/table-manager/) is achieved by
relying on the object store TTL feature, and will work for both
[boltdb-shipper](https://grafana.com/docs/loki/latest/operations/storage/boltdb-shipper/) store
and chunk/index store.

However retention through the
[Compactor](https://grafana.com/docs/loki/latest/operations/storage/boltdb-shipper/#compactor) is
supported only with the
[boltdb-shipper](https://grafana.com/docs/loki/latest/operations/storage/boltdb-shipper/) store.

Set `retention_enabled` to true. Without this, the Compactor will only compact tables.

Marked chunks will only be deleted after `retention_delete_delay` configured is expired.

The minimum retention period is 24h, the default is 30 days.

Example with compactor:

```yaml
compactor:
  working_directory: /data/retention
  shared_store: gcs
  compaction_interval: 10m
  retention_enabled: true
  retention_delete_delay: 2h
  retention_delete_worker_count: 150
schema_config:
    configs:
      - from: "2020-07-31"
        index:
            period: 24h
            prefix: loki_index_
        object_store: gcs
        schema: v11
        store: boltdb-shipper
storage_config:
    boltdb_shipper:
        active_index_directory: /data/index
        cache_location: /data/boltdb-cache
        shared_store: gcs
    gcs:
        bucket_name: loki
```

## Install Logging driver for Docker

```bash
docker plugin install grafana/loki-docker-driver:2.9.1 --alias loki --grant-all-permissions
```

## Set Loki as default logging driver in Docker

Edit `/etc/docker/daemon.json`:

```yaml
{
    "debug" : true,
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "https:// <user> : <pass> @log.company.nl/loki/api/v1/push",
        "loki-batch-size": "400"
    }
}
```

## Configure as default logging driver with docker-compose

Add this to the container definition:

```yaml
    logging:
      driver: loki
      options:
        loki-url: "https://loki.aike.be/loki/api/v1/push"
```
