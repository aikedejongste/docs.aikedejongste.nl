---
layout: default
title: Sensors
has_children: false
---

# Hardware and sensors

## Get current power consumption in watt

```bash
cat /sys/class/power_supply/BAT0/power_now | awk '{print $1*10^-6 " W"}'
```

## Get current battery info

```bash
upower -i /org/freedesktop/UPower/devices/battery_BAT0
```

## Update current battery and charging info 

```bash
busctl call org.freedesktop.UPower /org/freedesktop/UPower/devices/battery_BAT0 org.freedesktop.UPower.Device Refresh
```
