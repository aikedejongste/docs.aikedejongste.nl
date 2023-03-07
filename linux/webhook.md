---
layout: default
title: Webook
has_children: false
parent: Linux
---

# Webook

The Debian/Ubuntu package called `webhook` is very useful. Uses systemd to run but you have to adjust the config a bit.

```bash
apt install webhook
```

The file `/lib/systemd/system/webhook.service` should contain this ExecStart command: 

```
ExecStart=/usr/bin/webhook -nopanic -hooks /etc/webhook.conf -hotreload -verbose -port 8000
```

This works but use a nicer way to do this next time. Editing this file directly is not recommended.

Here is a `/etc/webhook.conf` example:

```json
[
  {
    "id": "deploy-j5wnuY96pbtit",
    "execute-command": "deploy.sh",
    "command-working-directory": "/opt/app-name/"
  }
]
```

## Firewall on Digital Ocean

```bash
# to get the Droplet ID
doctl compute droplet  list
# add firewall rule
doctl compute firewall create --name "allow-webhook-from-all" --inbound-rules "protocol:tcp,ports:9000" --droplet-ids
```

## Github Actions line

```yaml
    - name: Trigger deploy webhook
      run: curl -s -k http://host.name.com:9000/hooks/deploy-WFUEmasdfasdfasfdk
```


# Deploy script for containers

```bash
#!/bin/bash
#set -o errexit
#set -o nounset
#set -o pipefail

# load file outside of git repo with your DOCKER_TOKEN (PAT)
source ~/.env
# login so we can pull the container
echo $DOCKER_TOKEN | docker login ghcr.io --username doesnt@matter.com --password-stdin
# actually pull container
docker-compose pull backend

# restart container
# docker-compose restart backend # WARN: this seems to load an old image
docker compose stop backend
docker-compose up -d --force-recreate --renew-anon-volumes backend # https://stackoverflow.com/a/50059206/159086
```
