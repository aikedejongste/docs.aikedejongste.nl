---
layout: default
title: Terraform Hetzner
parent: Cloud Infrastructure
has_children: false
---

# Terraform Hetzner

## Single Debian VPS with firewalll

- TODO: add DNS for hostname
- TODO: name firewall rules

```yaml
terraform {
  required_providers {
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

# Fetch SSH key
data "http" "aikedejongste_ssh_key" {
  url = "https://github.com/aikedejongste.keys"
}

# Create SSH key
resource "hcloud_ssh_key" "aikedejongste" {
  name       = "aikedejongste"
  public_key = chomp(data.http.aikedejongste_ssh_key.response_body)
}

resource "hcloud_server" "demovps" {
  name         = "demovps"
  server_type  = "cx31"
  image        = "debian-12"
  ssh_keys     = [hcloud_ssh_key.aikedejongste.id]
  location     = "fsn1"
  firewall_ids = [hcloud_firewall.demovps_firewall.id]
  backups      = false
}

# resource "hcloud_volume" "extra_storage" {
#   name        = "extra_storage"
#   size        = 100
#   location    = "fsn1"
# }
# 
# resource "hcloud_volume_attachment" "extra_storage_attachment" {
#   server_id = hcloud_server.demovps.id
#   volume_id = hcloud_volume.extra_storage.id
# }

resource "hcloud_firewall" "demovps_firewall" {
  name = "demovps_firewall"

  rule {
    direction = "in"
    protocol  = "tcp"
    port      = "22"
    source_ips = [
      "1.2.3.4/32",
      "5.6.7.8/32"
    ]
  }
}

resource "local_file" "inventory" {                                             
  filename = "./inventory.ini"                                                  
  content  = templatefile("inventory.tpl", {                                    
    demovps = hcloud_server.demovps.ipv4_address
  })                                                                            
}
```
