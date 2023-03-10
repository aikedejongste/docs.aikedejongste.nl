---
layout: default
title: Netplan private ip
parent: Networking
---

# Netplan private ip only configs


## Disable cloud-init network config

### With Ansible:
```
- name: Disable cloud-init network config
  copy:
    dest: "/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg"
    content: |
      network: {config: disabled}
```

### With CLI:
```bash echo "network: {config: disabled}" > /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg```

## General suggestions

In some versions use this for IP address:
```
      addresses: [192.168.11.13/24]
```

In some versions you have to use default for the route:
```
      routes:
        - to: default
          via: 192.168.11.1
```

## Netplan Hetzner private only config

This doesn't work for some reason. Suggestions welcome.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens10:
      optional: false
      addresses: 
      - 10.10.1.6/32
      routes:
      - to: 0.0.0.0/0
        via: 10.10.1.1
        on-link: true
      nameservers:
        addresses: [8.8.8.8]
      dhcp4: no
      dhcp6: no
```

## Netplan private IP with Ansible

Useful vars:
```
tasks:
  - debug: var=ansible_all_ipv4_addresses
  - debug: var=ansible_default_ipv4.address

hostvars[inventory_hostname]['ansible_default_ipv4']['address']
```

```yaml


- role: mrlesmithjr.netplan
  #when: ansible_distribution == 'Ubuntu' and ansible_distribution_release == 'jammy'
  # inventory must look like:
  # [webservers]
  # public.hostname.com ansible_user=root internal_ip=192.168.1.2
  become: yes
  netplan_enabled: true
  netplan_config_file: /etc/netplan/01-netcfg.yaml
  netplan_renderer: networkd
  # Configuration defined bellow will be written to the file defined above in `netplan_config_file`.
  netplan_configuration:
    network:
      version: 2
      renderer: networkd
      ethernets:
        ens4:
          optional: true
          addresses: ["{{ hostvars[inventory_hostname].internal_ip }}/24"]
          nameservers:
            addresses: [8.8.8.8]
          dhcp4: no
          dhcp6: no
```

