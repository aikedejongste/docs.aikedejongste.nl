---
layout: default
title: DO Functions
has_children: false
parent: Programming
---

# Digital Ocean functions

## Authentication and accounts

List accounts with: `doctl auth list`

Switch accounts with: `doctl auth switch --context "my team"`

## Serverless
1. doctl serverless install
2. doctl serverless connect
3. doctl serverless status
4. doctl serverless deploy .

## New function

- `doctl serverless init do-function-registry-garbage/ --overwrite`
