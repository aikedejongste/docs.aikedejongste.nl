---
layout: default
title: Backups
has_children: true
---

# Backups

## zip every directory in the current directory and then delete the original directories:

```bash for d in */; do zip -r "${d%/}.zip" "$d" && rm -r "$d"; done```

