---
layout: default
title: Devcontainers
parent: Editors
---

# Devcontainers

## .devcontainer.json

```json
{
  "name": "Ubuntu",
  "image": "mcr.microsoft.com/devcontainers/base:jammy",
  "features": {
    "ghcr.io/devcontainers-contrib/features/ansible:2": {},
    "ghcr.io/devcontainers-contrib/features/apt-packages:1": {
                    "clean_ppas": true,
                    "preserve_apt_list": true,
                    "packages": "htop, vim, nmap, tmux, w3m, yamllint, docker.io, docker-compose, pip, file, ansible-lint, age, mlocate",
                    "ppas": "ppa:deadsnakes/ppa"
    },
    "ghcr.io/devcontainers/features/docker-in-docker:2.2.0": {},
    "ghcr.io/devcontainers/features/kubectl-helm-minikube:1": {},
    "ghcr.io/devcontainers-contrib/features/pipx-package:1": {
                    "includeDeps": true,
                    "package": "black",
                    "version": "latest",
                    "injections": "pylint pytest",
                    "interpreter": "python3"
    },
    "ghcr.io/devcontainers-contrib/features/sops:1": {},
    "ghcr.io/devcontainers-contrib/features/terraform-asdf:2": {},
    "ghcr.io/stuartleeks/dev-container-features/shell-history:0": {},
    "ghcr.io/natescherer/devcontainers-custom-features/k9s:1": {},
    "ghcr.io/lukewiwa/features/shellcheck:0": {}
  },
  "customizations": {
    "vscode": {
      "extensions": [
                      "Tim-Koehler.helm-intellisense",
                      "ms-kubernetes-tools.vscode-kubernetes-tools",
                      "GitHub.copilot",
                      "DavidAnson.vscode-markdownlint", // better with a config file
                      "4ops.terraform", // probably choose 4ops or HashiCorp
                      "HashiCorp.terraform",
                      "redhat.vscode-yaml",
                      "GitHub.copilot-chat",
                      "njzy.stats-bar",
                      "betajob.modulestf", // terraform
                      "shakram02.bash-beautify",
                      "Trunk.io" // linter for everything
                      // "redhat.ansible", // includes Ansible-lint
      ]
    }
  },
  "postCreateCommand": "ansible-galaxy collection install kubernetes.core ansible.posix community.general community.sops community.postgresql"
}


```
