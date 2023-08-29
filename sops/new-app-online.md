---
layout: default
title: SOP - webapp online
parent: SOPs
---

# Webapp Online

{: .highlight }
This is just for the infra part of the business.

## External checks

- [ ] is the SSL cert at least a B+ [SSLlabs](https://ssllabs.com/ssltest/analyze.html)
- [ ] is uptime monitoring in place and assigned to the right person

## Configuration checks

- [ ] firewall configured?
- [ ] SSH key only
- [ ] SSH from dedicated IP only
- [ ] do you, the owner, have access to backups
- [ ] are backups at least on another server at another provider?
- [ ] security.txt
- [ ] security headers
- [ ] webserver version hidden
- [ ] DNS CAA on
- [ ] server security updates automatic
- [ ] Rate limit in webserver
- [ ] logging to external
- [ ] vnstat

## Monitoring checks

- [ ] disk usage monitor
- [ ] memory usage monitor
- [ ] Lynis -> health checks
