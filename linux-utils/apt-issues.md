---
layout: default
title: APT-issues
has_children: false
parent: Linux
---

# APT-issues

## Requirement you need to install

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg
```

apt-transport-https: This package allows the use of repositories accessed via the HTTPS protocol, which is more secure than HTTP.
ca-certificates: This package contains the common CA certificates, which are used to verify the authenticity of SSL/TLS connections.
software-properties-common: This package provides an abstraction of the used apt repositories, allowing you to easily manage them. Also provides lsb_release command.
gnupg: This package provides the GNU Privacy Guard, which is used to sign and verify packages and repositories.

## Don't pipe

When this doesn't work:

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
```

Do this instead:

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor -o /usr/share/keyrings/helm.gpg
```
