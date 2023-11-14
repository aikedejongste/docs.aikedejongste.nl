---
layout: default
title: SOP - app in container
parent: SOPs
---

# Webapp Online

{: .highlight }
This should be a Github Actions workflow

## External checks

- [ ] is the SSL cert at least a B+ [SSLlabs](https://ssllabs.com/ssltest/analyze.html)
- [ ] is uptime monitoring in place and assigned to the right person

## Configuration checks

- [ ] no secrets in container

## Monitoring checks

- [ ] disk usage monitor
- [ ] memory usage monitor
- [ ] Lynis -> health checks
