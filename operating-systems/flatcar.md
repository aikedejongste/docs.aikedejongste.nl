---
layout: default
title: Flatcar
has_children: false
parent: Operating Systems
---

# Flatcar Linux

cat example.yaml | docker run --rm -i quay.io/coreos/butane:release > /var/lib/libvirt/flatcar-linux/flatcar-linux1/provision.ign


sudo flatcar-install -d /dev/sda -i fc.json


## Docker-compose

```yaml
storage:
  files:
    - path: /opt/bin/docker-compose
      contents:
        source: https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64
        verification:
          hash: sha512-8ad55ea1234e206ecad3e8ecf30f93de5cc764a423bf8ff179b25320f148e475b569ab9ec68d90eacfe97269528ff8fef1746c05381c7d05edc85d2f2c245e69
      mode: 0755
    - path: /home/core/docker-compose.yml
      contents:
        inline: |
          services:
            web:
              image: nginx
              ports:
                - 80:80
      mode: 0644
      user:
        name: core
      group:
        name: core
systemd:
  units:
    - name: application.service
      enabled: true
      contents: |
        [Unit]
        Description=Minimalist docker-compose example
        [Service]
        ExecStart=/opt/bin/docker-compose -f /home/core/docker-compose.yml up
        [Install]
        WantedBy=multi-user.target
```
