---
layout: default
title: SOP - New Helm Chart
parent: SOPs
---

# Information required for new Helm Chart

## Required information:

- What is the name of the application?
- What is the name of the container? (Click < here > for instructions on creating a container first)
- Which required environment variables need to be set before this container can run?
- Does this application need an Ingress? Should it be accessible from outside of the cluster?
- What are the urls for the health checks?


## Optional information:

- How many replicas should be started?
- What tag or version should be deployed by default? `latest`?
- Does this application require external authentication with OAuth or something like it?
- Where is the documentation of the optional environment variables?
- Does this application need a Dapr config?
- Does this application need a database? And what type and how is it configured?
- What are the resource limits for this application?
- Does this need a pull secret?

