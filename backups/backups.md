---
layout: default
title: Backups
has_children: true
---

# Backups

## zip every directory in the current dir

```bash
for d in */; do zip -r "${d%/}.zip" "$d" && rm -r "$d"; done
```

## unzip every directory in the current dir

```bash
 .............
```

## Simple daily zip script

```
#!/bin/bash
DATE=`date +%d`
DIR=/Users/aike/Dropbox/Backups
cd /Users/aike/code/ && for i in */; do
  zip -r "$DIR/$DATE.${i%/}.zip" "$i";
done
```
