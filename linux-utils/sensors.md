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
or
```bash
while true; do cat /sys/class/power_supply/BAT0/power_now | awk '{print $1*10^-6 " W"}'; sleep 2; done
```

## Get current battery info

```bash
upower -i /org/freedesktop/UPower/devices/battery_BAT0
```

## Update current battery and charging info

```bash
busctl call org.freedesktop.UPower /org/freedesktop/UPower/devices/battery_BAT0 org.freedesktop.UPower.Device Refresh
```

## Restrict charging to 90%

Related link: [here](https://shallowsky.com/blog/linux/laptop/lenovo-charge-limiting.html)

`charge_start_threshold` is the legacy API

`charge_control_start_threshold` is the current API

```bash
apt install tlp

echo 90 > /sys/class/power_supply/BAT0/charge_control_start_threshold
echo 90 > /sys/class/power_supply/BAT0/charge_control_end_threshold
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


## Show Intel iGPU usuage

```bash
sudo intel_gpu_top
```

Alternatives I haven't tried are nvtop and glances.
