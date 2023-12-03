---
layout: default
title: Pihole
has_children: false
parent: Self Hosted Apps
---

# Pihole

## Caddyfile

```yaml
ph.you.ts.net {
  tls /config/pi.you.ts.crt /config/pi.you.ts.key
  redir / /admin
  reverse_proxy pihole:80
}
```

## Docker compose

```yaml
version: "3"

services:
  caddy:
    container_name: caddy
    image: caddy:latest
    networks:
      - caddy-net  # Network exclusively for Caddy-proxied containers
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"  # QUIC protocol support: https://www.chromium.org/quic/
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile  # config file on host in same directory as docker-compose.yml for easy editing.
      #- $PWD/site:/srv  # Only use if you are serving a website behind caddy
      - ./caddy_data:/data  # Use docker volumes here bc no need to access these files from host
      - ./caddy_config:/config  # Use docker volumes here bc no need to access these files from host
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    networks:
      - caddy-net  # Network exclusively for Caddy-proxied containers
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "8180:80/tcp"
    environment:
      TZ: 'Europe/Amsterdam'
      WEBPASSWORD: '.......'
    volumes:
      - './pihole/etc:/etc/pihole'
      - './pihole/dnsmasq:/etc/dnsmasq.d'
    #cap_add:
    #  - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    restart: unless-stopped

networks:
  caddy-net:
    driver: bridge
    name: caddy-net
```
