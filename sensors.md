---
layout: default
title: Hardware & sensors
has_children: false
---

# Hardware and sensors

## Get power consumption in watt

```bash
cat /sys/class/power_supply/BAT0/power_now | awk '{print $1*10^-6 " W"}'
```
