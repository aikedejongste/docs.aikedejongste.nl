---
layout: default
title: Ubuntu Desktop
has_children: false
parent: Operating Systems
---

# Ubuntu Desktop

## Install Syncthing

[Syncthing install instructions](apps/syncthing.md)

## Install Brave

```bash
sudo curl -fsSLo /etc/apt/sources.list.d/brave-browser-release.sources https://brave-browser-apt-release.s3.brave.com/brave-browser.sources
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
sudo apt update && sudo apt install -y brave-browser
```

## Install Signal

```bash
wget -O- https://updates.signal.org/desktop/apt/keys.asc | gpg --dearmor > signal-desktop-keyring.gpg
cat signal-desktop-keyring.gpg | sudo tee /usr/share/keyrings/signal-desktop-keyring.gpg > /dev/null
echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/signal-desktop-keyring.gpg] https://updates.signal.org/desktop/apt xenial main' |\
sudo tee /etc/apt/sources.list.d/signal-xenial.list
sudo apt update && sudo apt install signal-desktop
```

## Installl AntiGravity

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://us-central1-apt.pkg.dev/doc/repo-signing-key.gpg | \
  sudo gpg --dearmor --yes -o /etc/apt/keyrings/antigravity-repo-key.gpg
echo "deb [signed-by=/etc/apt/keyrings/antigravity-repo-key.gpg] https://us-central1-apt.pkg.dev/projects/antigravity-auto-updater-dev/ antigravity-debian main" | \
  sudo tee /etc/apt/sources.list.d/antigravity.list > /dev/null
sudo apt update && sudo apt install antigravity
```

## Install scripts / automation

- [DHH - Ubuntu Desktop Setup](https://gist.github.com/dhh/159b129f511f76db3ae8adb463f70d05)

## Launcher

[ulauncher.io](https://ulauncher.io/)

```bash
sudo add-apt-repository universe -y && sudo add-apt-repository ppa:agornostal/ulauncher -y && sudo apt update && sudo apt install ulauncher
```

## Gnome Extensions

### Disable updater notifications

```bash
gsettings set com.ubuntu.update-notifier no-show-notifications true
```

### Tactile

A window tiling extension for GNOME Shell.

- [Gitlab Tactile](https://gitlab.com/lundal/tactile)
- [Gnome Extension install page](https://extensions.gnome.org/extension/4548/tactile/)

```bash
sudo apt install -y gnome-browser-connector
open https://chromewebstore.google.com/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep
open https://extensions.gnome.org/extension/4548/tactile/
```
