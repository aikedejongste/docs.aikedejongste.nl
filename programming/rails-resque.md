---
layout: default
title: Rails - Resque
has_children: false
parent: Programming
---

# Rails - Resque

# rails #ruby

List queues: `Resque.queues`

All queues stats: `Resque.info`

Specific status stats: `Resque.info[:failed]`

Resque::Plugins::Status::Hash.get(zipper_worker_uuid)

Then follow progress with `pp Resque::Plugins::Status::Hash.get(uuid)`
