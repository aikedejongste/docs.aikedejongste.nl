---
layout: default
title: Rsync
has_children: false
parent: Linux
---

# Rsync

## Notes

- the `/` at the end of the source dir signal that you want to copy the contents, not the dir itself.

## Options

- -P = progress and patial
- -a = archive

## Local sync

```bash
rsync -aP /opt/source/ /opt/destination
```
