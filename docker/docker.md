---
layout: default
title: Docker
has_children: true
---

# Docker Installation

## Links

- [Docker Rollout](https://github.com/Wowu/docker-rollout)

## Related

- [Docker Logging to Loki](https://docs.aikedejongste.nl/logging/loki.html)

## Cleanup cronjob

```bash
0 4 * * * /usr/bin/docker container prune -f && /usr/bin/docker image prune -f
```

## Install from distro APT

```bash
apt install -y gnupg2 pass apparmor-utils docker.io docker-compose docker-compose-plugin
```

## Docker stop all

```bash
docker stop $(docker ps -a -q)

or

docker ps -q | % { docker stop $_ }
```

## Search trough logs

```bash
docker logs --since "$(date -d 'today 13:30' +'%Y-%m-%dT%H:%M:%S')" --until "$(date -d 'today 13:40' +'%Y-%m-%dT%H:%M:%S')" prodapp
```

## Manually cleanup old logs

As root, not with sudo!

```bash
truncate -s 0 /var/lib/docker/containers/**/*-json.log
```

## Install from Docker APT

```bash
sudo apt-get update && sudo apt-get install ca-certificates curl && sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc && sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

## Remove all containers from a host

```bash
docker rm -f $(docker ps -qa)
```

## Remove all volumes from a host

```bash
docker volume rm $(docker volume ls -q)
```

## Login with PAT

Create a new Personal Access Token [here](https://github.com/settings/tokens/new).

```bash
export PAT=.....
echo $PAT | docker login ghcr.io --username doesnt@matter.com --password-stdin

echo $PAT | docker login registry.gitlab.com -u aiketestuser1 --password-stdin
```

or

```bash
echo "ghp_REPLACE_ME" | docker login ghcr.io --username doesnt@matter.com --password-stdin
```

## Filter docker ps columns

Link: [docker docs](https://docs.docker.com/engine/reference/commandline/ps/#format)

```bash
{% raw %} docker ps -a --format="table {{.Names}}\t{{.Ports}}\t{{.CreatedAt}}\t{{.Status}}\t{{.Mounts}}" {% endraw %}
```

## Run sleep in a container that won't start

```bash
docker run --rm --entrypoint sh ubuntu:latest -c "sleep infinity"
```

## Log rotation

In /etc/docker/daemon.json:

```yaml
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m",
    "max-file": "5"
  }
}
```

or use Logrotate!

```yaml
/var/lib/docker/containers/*/*.log {
  rotate 7
  daily
  compress
  missingok
  delaycompress
  copytruncate
}
```

## Show log file sizes

```bash
sudo find /var/lib/docker/containers/ -name *-json.log -exec ls -lh {} \;
```

## Check log rotation status for container

```bash
docker inspect -f '{{.HostConfig.LogConfig}}' your_container
```

## Truncate logfile manually

If you don't want to recreate the container.

```bash
truncate -s 0 /var/lib/docker/containers/<container-id>/<container-id>-json.log
```

## Docker prune

```bash
docker image prune -af --filter "until=24h"
```
