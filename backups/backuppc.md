---
layout: default
title: BackupPC
parent: Backups
---

# BackupPC

compose file:

```yaml
version: '3.7'
services:
  caddy:
    container_name: caddy
    image: caddy:latest
    restart: unless-stopped
    ports:
      - "443:443"
      - "443:443/udp"
    volumes:
      - /opt/backuppc/Caddyfile:/etc/caddy/Caddyfile
      - /opt/backuppc/caddy_data:/data
      - /opt/backuppc/caddy_config:/config
    networks:
      - proxy

  backuppc-app:
    image: tiredofit/backuppc:5.3.14
    container_name: backuppc-app
    volumes:
      - /var/lib/backuppc:/var/lib/backuppc
      - ./conf/etc/:/etc/backuppc
      - ./conf/home/:/home/backuppc
      - ./logs:/www/logs
    environment:
      - CONTAINER_NAME=backuppc-app
      - BACKUPPC_UUID=10000
      - BACKUPPC_GUID=10000
      - NGINX_AUTHENTICATION_TYPE=BASIC
      - NGINX_AUTHENTICATION_BASIC_USER1=backuppc
      - NGINX_AUTHENTICATION_BASIC_PASS1=whatever-something
      - DEBUG_MODE=FALSE
    networks:
      - proxy
    restart: always
    extra_hosts:
       - "some-hostname:1.2.3.4"
networks:
  proxy:
    external: true
```

Caddyfile:

```
backup.company.nl
reverse_proxy backuppc-app:80
tls /config/cert.pem /config/cert.key
```

## Configure hosts with Ansible

```yaml
    - name: Add the backuppc user
      ansible.builtin.user:
        name: backuppc
        shell: /bin/bash
        groups: sudo
        append: yes

    - name: backuppc sudo for rsync only
      ansible.builtin.lineinfile:
        path: /etc/sudoers.d/backuppc
        state: present
        create: yes
        line: "backuppc ALL=NOPASSWD: /usr/bin/rsync"
        owner: root
        group: root

    - name: Set authorized key for backuppc
      authorized_key:
        user: backuppc
        state: present
        key: '{{ item }}'
      with_file:
        - keys/backuppc.pub
```

## Add hosts to known hosts and then to BackupPC config

```yaml
- name: BackupPC setup
  hosts: all (not backuppc host)
  tasks:
    - name: Keep a record of SSH host keys because of reinstalls
      delegate_to: localhost
      lineinfile:
        dest: mwp_known_hosts
        create: yes
        state: present
        line: "{{ lookup('pipe', 'ssh-keyscan -trsa ' + inventory_hostname) }}"
      ignore_errors: true

    - name: Add this host to BackupPC config
      delegate_to: localhost
      lineinfile:
        dest: backuppc-hosts
        create: yes
        state: present
        line: "{{ inventory_hostname + '  ' + '0' + ' ' + 'backuppc' }}"
```

## Configure BackupPC

```yaml
- name: BackupPC setup
  hosts: backuppc
  tasks:
    - name: Copy the known hosts to backuppc server so backuppc knows it connects to the right host
      become: yes
      ansible.builtin.copy:
        src: mwp_known_hosts
        dest: /opt/backuppc/conf/home/.ssh/known_hosts
        owner: 10000
        group: 10000
        mode: '0600'
        backup: yes

    - name: Copy the target hosts list to backuppc server to make sure all hosts have backups
      become: yes
      ansible.builtin.copy:
        src: backuppc-hosts
        dest: /opt/backuppc/conf/etc/hosts
        owner: 10000
        group: 10000
        mode: '0640'
        backup: yes
```
