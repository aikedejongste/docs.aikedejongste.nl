---
layout: default
title: Lenovo
has_children: false
parent: Hardware
---

# Lenovo

## Thinkpad X1 carbon

- F1 is bios
- F12 is boot menu

Windows Wi-Fi Driver can be downloaded [here](https://www.intel.com/content/www/us/en/download/19351/windows-10-and-windows-11-wi-fi-drivers-for-intel-wireless-adapters.html).


## Thinkfan config

```yaml
sensors:
  - hwmon: /sys/class/hwmon/hwmon6/temp1_input
fans:
  - tpacpi: /proc/acpi/ibm/fan
levels:
  - speed: 0
    upper_limit: [50]

  - speed: 1
    lower_limit: [40]
    upper_limit: [60]

  - speed: 2
    lower_limit: [60]
    upper_limit: [70]

  - speed: 5
    lower_limit: [70]
```
