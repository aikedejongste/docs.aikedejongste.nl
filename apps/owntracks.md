---
layout: default
title: Owntracks
has_children: false
parent: Self Hosted Apps
---

# Owntracks

Hacky script to submit laptop location:

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
