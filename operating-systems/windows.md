---
layout: default
parent: Operating Systems
title: Windows
---

# Windows

## Download links

- [Wireguard](https://download.wireguard.com/windows-client/)
- [Firefox](https://www.microsoft.com/store/productId/9NZVDKPMR9RD)
- [Signal](https://signal.org/download/windows/)
- [Microsoft Office](https://setup.office.com/)
- [TailScale](https://pkgs.tailscale.com/stable/tailscale-setup-latest.exe)
- [VS Code](https://code.visualstudio.com/Download)
- [Git](https://git-scm.com/download/win)
- [SyncThing](https://github.com/Bill-Stewart/SyncthingWindowsSetup/?tab=readme-ov-file#download)

## Put Windows ISO on USB stick

On Ubuntu umount the file system first with `umount`. But do not eject the disk itself.

```bash
sudo dd if=Win11_22H2_EnglishInternational_x64v1.iso of=/dev/sdd bs=1M status=progress
```

## Disable sleeping when locked

Edit and set attributes = 2

```bash
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\238C9FA8-0AAD-41ED-83F4-97BE242C8F20\7bc4a2f9-d8fc-4469-b07b-33eb785aaca0
```

## Avoid space for quote

Go to "Time and Language" settings, click "Language options" on the installed language (the 3rd bar). And remove "US international" keyboard map and add "US Qwerty".

## Enable WSL

Open a PowerShell terminal with Administrator rights and paste this:

```shell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

## WSL ctrl-v in vim

Use `ctrl+shift+,` to open the WSL settings. And comment this line:

```json
    // { "command": {"action": "paste", ...}, "keys": "ctrl+v" }, <------ THIS LINE
```

## Map CapssLock to ESC on Windows 7,8 and 10

```bash
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,3a,00,46,00,01,00,3a,00,00,00,00,00
```

## Make a directory cAsE SeNsItive

```bash
fsutil.exe file SetCaseSensitiveInfo C:\folder\path enable
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

- Open Settings.
- Click on Personalization.
- Click on Start.
- Turn off "Show recently opened items in Jump Lists on Start....."

Quick tip: If you want to reset the view, turn the toggle switch off and on again.

## Disable automatic reboot after updates

Prevent Windows Update Automatic Restart using Registry Editor
Open the Registry Editor and navigate to the following key:

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`

If you don’t see it, create one. You may have to create \WindowsUpdate\AU.
Now under this key, create a new 32-bit DWORD called `NoAutoRebootWithLoggedOnUsers`
and give it a hexadecimal value data of 1.

This will prevent automatic reboot while users are logged on.

## Share WSL files with Windows

If you cannot see the files from a mount in WSL in the Windows Explorer this might help, but be careful!

```bash
C:\Windows\System32\cmd.exe /k %windir%\System32\reg.exe ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA /t REG_DWORD /d 0 /f
```

## Lenovo Vantage in MS store

Direct [link](https://apps.microsoft.com/store/detail/lenovo-vantage/9WZDNCRFJ4MV?hl=en-us&gl=us)

## Install without internet

On the “Oops, you've lost internet connection” or “Let's connect you to a network” page,
use the “Shift + F10” keyboard shortcut. In Command Prompt, type the OOBE\BYPASSNRO
command to bypass network requirements on Windows 11 and press Enter.
