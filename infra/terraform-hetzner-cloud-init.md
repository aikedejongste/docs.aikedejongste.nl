---
layout: default
title: TF Hetzner Cloud Init
parent: Cloud Infrastructure
has_children: false
---

# Terraform Hetzner with Cloud Init

## Single Debian VPS with firewalll

### Terraform file

```yaml
terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "~>2"
    }
    local = {
      source = "hashicorp/local"
      version = "~>2"
    }
    http = {
      source = "hashicorp/http"
      version = "~>3"
    }
    hcloud = {                                                                     
      source = "hetznercloud/hcloud"           
      version = "1.41.0"                                 
    }                                                                           
    hetznerdns = {                                                              
      source = "timohirt/hetznerdns"                                            
      version = "2.2.0"                                                         
    }                                                                           
  }                                                                             
  required_version = ">= 0.13"                                                  
}

variable "hetzner_token" {
  description = "Hetzner API token"
  type        = string
}

provider "hcloud" {
  token = var.hetzner_token
}

resource "hcloud_server" "camunda1" {
  name        = "camunda1"
  server_type = "cx21"
  image       = "debian-12"
  location    = "hel1"
  firewall_ids = [hcloud_firewall.camunda1_firewall.id]
  backups     = true
  user_data = file("cloud-init.yaml")
}

resource "hcloud_firewall" "camunda1_firewall" {
  name = "camunda1_firewall"

  rule {
    direction = "in"
    protocol  = "tcp"
    port      = "22"
    source_ips = [
      "1.1.1.1/32",
      "1.1.1.1/32"
    ]
  }
  
  rule {
    direction = "in"
    protocol  = "tcp"
    port      = "443"
    source_ips = [
      "1.1.1.1/32",
      "1.1.1.1/32"
    ]
  }

  rule {
    direction = "in"
    protocol  = "tcp"
    port      = "5000"
    source_ips = [
      "1.1.1.1/32",
      "1.1.1.1/32"
    ]
  }

}

resource "local_file" "inventory" {                                             
  filename = "./inventory.ini"                                                  
  content  = templatefile("inventory.tpl", {                                    
    camunda1 = hcloud_server.camunda1.ipv4_address
  })                                                                            
}  

```


### Cloud Init file

```yaml
#cloud-config
groups:
  - docker
users:
  - name: aikedejongste
    groups: sudo,docker
    sudo: ALL=(ALL) NOPASSWD:ALL
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMJZhBxjBZgaU5JQWaS2smXC9IFS46jR5jVdDYHyq8DS
package_update: true
package_upgrade: true
packages:
  - vim
  - git
  - vnstat
  - docker.io
  - nload
  - docker-compose
  - htop
  - screen
```


### TF output template

Put in `inventory.tpl`.

```yaml
[all]
${camunda1} ansible_user=root ansible_ssh_common_args='-o ForwardAgent=yes'
```
