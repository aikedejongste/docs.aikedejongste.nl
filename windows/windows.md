---
layout: default
title: Windows
---

# Windows

## Download links

* [Wireguard](https://download.wireguard.com/windows-client/)
* [Firefox](https://www.microsoft.com/store/productId/9NZVDKPMR9RD)
* [Signal](https://signal.org/download/windows/)
* [Microsoft Office](https://setup.office.com/)

## Disable sleeping when locked

Edit and set attributes = 2

```text
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\238C9FA8-0AAD-41ED-83F4-97BE242C8F20\7bc4a2f9-d8fc-4469-b07b-33eb785aaca0
```

## Avoid space for quote

Go to language settings and remove "US international" keyboard map and add "US Qwerty".

## Enable WSL

Open a PowerShell terminal with Administrator rights and paste this:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

## Map CapssLock to ESC on Windows 7,8 and 10

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,3a,00,46,00,01,00,3a,00,00,00,00,00
```


## Mount LUKS encrypted ext4 drive

In Powershell:

```bash
wmic diskdrive list brief
wsl --mount \\.\PHYSICALDRIVE1 --bare
```

In WSL2:

```bash
sudo blkid
sudo cryptsetup luksOpen /dev/sdd1 disk-name
sudo mount /dev/mapper/disk-name /mnt/somewhere
```

## Disable recent files in start menu

* Open Settings.
* Click on Personalization.
* Click on Start.
* Turn off "Show recently opened items in Jump Lists on Start....."

Quick tip: If you want to reset the view, turn the toggle switch off and on again.


## Disable automatic reboot after updates

Prevent Windows Update Automatic Restart using Registry Editor
Open the Registry Editor and navigate to the following key:

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`

If you don’t see it, create one. You may have to create \WindowsUpdate\AU.
Now under this key, create a new 32-bit DWORD called `NoAutoRebootWithLoggedOnUsers` and give it a hexadecimal value data of 1.

This will prevent automatic reboot while users are logged on.



