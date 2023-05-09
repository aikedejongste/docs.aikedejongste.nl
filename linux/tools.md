---
layout: default
title: CLI Tools
has_children: false
parent: Linux
---

# CLI Tools

## Glow (Markdown reader)
```bash
something
```

## K9s (Kubernetes GUI)
Bash

```bash
wget https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_amd64.tar.gz -O /tmp/k9s.gz
cd /tmp && tar -zxvf /tmp/k9s.gz && mv /tmp/k9s /usr/local/bin/k9s && chmod +x /usr/local/bin/k9s
```

Ansible
```yaml
    - name: Download latest k9s binary
      get_url:
        url: "https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_amd64.tar.gz"
        dest: /tmp/k9s.tar.gz
      tags: [pre-reqs]

    - name: Extract k9s binary
      unarchive:
        src: /tmp/k9s.tar.gz
        dest: /tmp
        remote_src: true
      tags: [pre-reqs]

    - name: Install k9s binary
      copy:
        src: /tmp/k9s
        dest: /usr/bin/k9s
        remote_src: yes
        mode: 'a+x'
      tags: [pre-reqs]

    - name: Remove downloaded files
      file:
        path: /tmp/k9s*
        state: absent
      tags: [pre-reqs]
```

## Detect-secrets

Link: [github repo](https://github.com/Yelp/detect-secrets)

```bash
pip install detect-secrets && echo -n "export PATH=\"~/.local/bin:$PATH\"" >> ~/.bashrc
```


