---
layout: default
title: Sensors and power
has_children: false
parent: Linux
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

## Gnome power settings

```bash
gsettings get org.gnome.settings-daemon.plugins.power < schema >
```

Available schemas:

- ambient-enabled
- idle-dim
- lid-close-battery-action
- power-button-action
- sleep-inactive-ac-timeout
- sleep-inactive-battery-timeout
- idle-brightness
- lid-close-ac-action
- lid-close-suspend-with-external-monitor
- power-saver-profile-on-low-battery
- sleep-inactive-ac-type
- sleep-inactive-battery-type

But usually Upower takes over, see: `/etc/UPower/UPower.conf`
