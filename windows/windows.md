---
layout: default
title: Windows
#parent: Terraform
---

# Windows

## Download links

[Wireguard](https://download.wireguard.com/windows-client/)
[Firefox](https://www.microsoft.com/store/productId/9NZVDKPMR9RD))

## Disable sleeping when locked:

Edit and set attributes = 2

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\238C9FA8-0AAD-41ED-83F4-97BE242C8F20\7bc4a2f9-d8fc-4469-b07b-33eb785aaca0 
``

## Enable WSL

Open a PowerShell terminal with Administrator rights and paste this:
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
