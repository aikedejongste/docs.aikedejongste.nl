---
layout: default
title: Windows
#parent: Terraform
---

# Windows

## Download links

* [Wireguard](https://download.wireguard.com/windows-client/)
* [Firefox](https://www.microsoft.com/store/productId/9NZVDKPMR9RD)
* [Signal](https://signal.org/download/windows/)

## Disable sleeping when locked:

Edit and set attributes = 2

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\238C9FA8-0AAD-41ED-83F4-97BE242C8F20\7bc4a2f9-d8fc-4469-b07b-33eb785aaca0 
```

## Avoid space for quote

Go to language settings and remove "US international" keyboard map and add "US Qwerty".

## Enable WSL

Open a PowerShell terminal with Administrator rights and paste this:
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

## Map CapssLock to ESC on Windows 7,8 and 10

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,3a,00,46,00,01,00,3a,00,00,00,00,00
```
