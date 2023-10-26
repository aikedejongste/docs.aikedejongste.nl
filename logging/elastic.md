---
layout: default
title: Elasticsearch
parent: Logging
---

# Elasticsearch

## Check credentials

```bash
curl -v -u "user:pass" https://elastic.company.com
```

Should return some json and "You know, for search".

## Install Filebeat

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt-get update && sudo apt-get install filebeat
sudo systemctl enable filebeat
```

## Filebeat autodiscover docker logs

```
filebeat.autodiscover:
  providers:
    - type: docker
      templates:
        - condition:
            contains:
              docker.container.image: nginx
          config:
            - type: log
              paths:
                - "/var/lib/docker/containers/${'$'}{data.docker.container.id}/*.log"
              exclude_lines: ["^\\s+[\\-`('.|_]"]  # drop asciiart lines

output.logstash:
   hosts: ["xxxx:5044"]
path:
  data: /appl/graylog-sidecar/collectors/filebeat/data
  logs: /appl/graylog-sidecar/collectors/filebeat/log
```
