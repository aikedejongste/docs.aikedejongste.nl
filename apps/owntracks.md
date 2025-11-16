---
layout: default
title: Owntracks
has_children: false
parent: Self Hosted Apps
---

# Owntracks

This runs Owntracks recorder as a systemd service and the viewer with Podman.

## Android app config

- Mode: http
- URL: https://ot.you.nl/owntracks/pub
- Device ID: s24plus
- Track ID: 2s
- Username: username in Caddyfile
- Password: password in Caddyfile

OTRC file that you can import in the Android app:

```
{
  "_type" : "configuration",
  "_id" : "58e501ca",
  "_build" : 420504022,
  "autostartOnBoot" : true,
  "cmd" : true,
  "connectionTimeoutSeconds" : 30,
  "debugLog" : false,
  "deviceId" : "s24plus",
  "discardNetworkLocationThresholdSeconds" : 0,
  "dontReuseHttpClient" : false,
  "enableMapRotation" : true,
  "encryptionKey" : "",
  "experimentalFeatures" : [ ],
  "extendedData" : true,
  "fusedRegionDetection" : true,
  "ignoreInaccurateLocations" : 0,
  "ignoreStaleLocations" : 0.0,
  "locatorDisplacement" : 100,
  "locatorInterval" : 600,
  "mapLayerStyle" : "GoogleMapHybrid",
  "mode" : 3,
  "monitoring" : 1,
  "moveModeLocatorInterval" : 30,
  "notificationEvents" : true,
  "notificationGeocoderErrors" : true,
  "notificationHigherPriority" : false,
  "notificationLocation" : true,
  "opencageApiKey" : "",
  "osmTileScaleFactor" : 1.0,
  "password" : "yourpassword",
  "pegLocatorFastestIntervalToInterval" : false,
  "ping" : 15,
  "publishLocationOnConnect" : false,
  "remoteConfiguration" : false,
  "reverseGeocodeProvider" : "Device",
  "showRegionsOnMap" : true,
  "theme" : "Auto",
  "tid" : "2s",
  "url" : "https://ot.you.nl/owntracks/pub",
  "username" : "yourusername"
}
``` 

## Debian repo

```
/etc/apt/sources.list.d/owntracks.list 
deb  http://repo.owntracks.org/debian bookworm main
```

## Config file

```
#/etc/default/ot-recorder

OTR_HTTPPREFIX="https://ot.you.nl/owntracks"
```


## Systemd config

```bash
# /etc/systemd/system/ot-recorder.service

[Unit]
Description=OwnTracks Recorder
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=owntracks
WorkingDirectory=/
ExecStartPre=/bin/sleep 3
ExecStart=/usr/sbin/ot-recorder

[Install]
WantedBy=multi-user.target
```

## Override

```bash
# /etc/systemd/system/ot-recorder.service.d/override.conf 
[Service]
ExecStart=
ExecStart=/usr/sbin/ot-recorder --host '' --port ''
```

## Env

Probably not used:

```bash
#/etc/ot-recorder.env 
OTR_HTTPPREFIX https://ot.you.nl/owntracks/
```

## Caddy for https

```
#/etc/caddy/Caddyfile 
ot.you.nl {
	basicauth {
		yourusername $2a$14$dY97L4nZXPvTXYF5............
	}
        handle /owntracks* {
          uri strip_prefix /owntracks
          reverse_proxy http://127.0.0.1:8083
        }
	reverse_proxy localhost:8888
}
```
## Viewer

```
#/opt/owntracks-frontend# cat podman-compose.yaml 
version: "3"

services:
  owntracks-frontend:
    image: docker.io/owntracks/frontend 
    ports:
      - 8888:80
    volumes:
      - /opt/owntracks-frontend/config.js:/usr/share/nginx/html/config/config.js
    environment:
      - SERVER_HOST=ot.you.nl
      - SERVER_PORT=443
      - API_BASEURL=owntracks
    restart: unless-stopped
```

## Viewer config

```
#root@owntracks:/opt/owntracks-frontend# cat config.js 

window.owntracks = window.owntracks || {};
window.owntracks.config = {
  api: {
    baseUrl: "https://ot.you.nl/owntracks/",
  },
};
```

## Hacky script to submit laptop location:

```bash
#!/bin/bash
echo "Location script started."
export PATH=/usr/local/bin:/usr/bin:/bin

OT_USER=.....
OT_DEVICE="$(hostname)"
OT_HOST=.....
OT_PASS=.....

# Get battery percentage or default to 100
if [ -f /sys/class/power_supply/BAT0/capacity ]; then
    batt=$(cat /sys/class/power_supply/BAT0/capacity)
else
    batt=100
fi

# Get lat/lon from GeoIP
read lat lon <<<$(curl -s https://ipapi.co/json | jq -r '"\(.latitude) \(.longitude)"')
tst=$(date +%s)

# Send to OwnTracks
curl -s -u "$OT_USER:$OT_PASS" -X POST \
  -H "Content-Type: application/json" \
  -d "{\"_type\":\"location\",\"lat\":$lat,\"lon\":$lon,\"tst\":$tst,\"batt\":$batt}" \
  "https://$OT_HOST/owntracks/pub?u=$OT_USER&d=$OT_DEVICE"
```
