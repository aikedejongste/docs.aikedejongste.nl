---
layout: default
title: VSCode
parent: Editors
has_children: false
---

# VSCode

And editors based on VSCode like Windsurf and Antigravity.

## Installation

See [operating-systems/ubuntu-desktop.md](operating-systems/ubuntu-desktop.md)

## Extensions

```bash
#!/bin/bash
extensions=(
  ms-kubernetes-tools.vscode-kubernetes-tools
  hashicorp.terraform
  redhat.vscode-yaml
  gitlab.gitlab-workflow
  ms-azuretools.vscode-docker
)

for ext in "${extensions[@]}"; do
  antigravity --install-extension "$ext"
done
```


## Set CAPS-LOCK to ESC

```bash
for d in "Code" "Code - OSS" "Antigravity"; do f="$HOME/.config/$d/User/settings.json"; if [ -f "$f" ]; then tmp=$(mktemp); jq '."keyboard.dispatch" = "keyCode"' "$f" > "$tmp" && mv "$tmp" "$f" && echo "Updated $f"; fi; done
```