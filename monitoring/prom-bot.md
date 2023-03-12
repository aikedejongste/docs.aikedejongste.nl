---
layout: default
title: Prometheus bots
has_children: false
parent: Monitoring
---

# Notifications to Mattermost

* [AlertManager plugin](https://github.com/cpanato/mattermost-plugin-alertmanager/blob/main/README.md)

# Prometheus Telegram bot

In config.yaml

```yaml
telegram_token: "123456789.........."
# ONLY IF YOU USING DATA FORMATTING FUNCTION, NOTE for developer: important or test fail
#time_outdata: "02/01/2006 15:04:05"
template_path: "template.tmpl" # ONLY IF YOU USING TEMPLATE
time_zone: "Europe/Amsterdam" # ONLY IF YOU USING TEMPLATE
split_msg_byte: 4000
send_only: true # use bot only to send messages.
```

In template.tmpl

```yaml
Alertname: {{ .CommonLabels.alertname }}
Status:  {{ .Status }}
Instance: {{ .CommonLabels.instance }}
Runbook: {{ .CommonLabels.runbook }}
````

AlertManger config:

```yaml
global:
  resolve_timeout: 5m
route:
  group_by: ['alertname']
  group_wait: 30s
  group_interval: 30s
  repeat_interval: 1d
  receiver: 'web.hook'
receivers:
- name: 'web.hook'
  webhook_configs:
  - url: 'http://prometheus-bot:9087/alert/-123123123'
inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'dev', 'instance']
```


