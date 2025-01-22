---
layout: default
title: Wifi
parent: Networking
---

# Wifi

```bash
nmcli
```

## Get saved passwords in Ubuntu

```bash
cd /etc/NetworkManager/system-connections
sudo grep -r '^psk=' .
```

## DFS

Daarbij bevindt een deel van de frequentiegebieden waar de eerder genoemde 19 kanalen te vinden zijn zich in een niet-exclusief deel van het spectrum. Dit gebied wordt bijvoorbeeld ook gebruikt door radarsystemen, die voorrang hebben op wifi-netwerken van thuisgebruikers. Deze zogenaamde dynamic frequency selection (dfs) kanalen mag je gebruiken, zolang ze niet gebruikt worden voor andere zaken. Zodra je router anders detecteert, moet hij onmiddellijk het desbetreffende kanaal verlaten. Dat betekent dat als je in de eerste plaats een betrouwbaar netwerk moet hebben, je zonder de dfs-kanalen nog maar 4 kanalen van 20 MHz, 2 van 40 MHz of slechts 1 van 80 MHz overhoudt.

 In 5GHz heb je namelijk 4 kanalen die je volledig vrij mag gebruiken en 15 kanalen die mogelijk door radars gebruikt worden en die je alleen mag gebruiken als je DFS gecertificeerd bent. Vervolgens is het zo dat de laagste 8 kanalen in 5GHz een max zendvermogen van 200mW kennen, terwijl je bij de hoogste 11 kanalen met 1000mW mag zenden

 - kun je in 5GHz alleen kanalen 36, 40, 44 en 48 selecteren, dan geen DFS support = FOUT
- kun je in 5GHz ook kanalen 52-64 en >100 selecteren, dan wel DFS support = GOED

6GHz fixes this

10,000's of AP's.  In 90 days time, we found exactly 2 AP's that briefly detected DFS traffic causing a controlled transition.  Cisco, a major WiFi vendor, as well as many industry vendors, use a statistic that DFS accounts for way less than 2% (per network, and not WLAN) of wireless traffic.

TP-Link EAP615-Wall does not support DFS!

## OFDMA

Link: [Tweakers article](https://tweakers.net/reviews/8814/8/hoe-werkt-wifi-van-modulatie-tot-mu-mimo-wi-fi-6-ofdma-en-mu-mimo.html)

## Target wake time
