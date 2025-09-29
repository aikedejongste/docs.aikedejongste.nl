---
layout: default
title: Find
has_children: false
parent: Linux
---

# Bash

## Find duplicate files

```bash
fdupes -r dir123/
```

## Find and delete empty files

```bash
find . -type f -empty -print -delete
```

## Find and delete old files

```bash
/usr/bin/find /var/backups/* -mtime +100 -name starts-with-* -type f -prune -exec rm -rf {} \;
```

## Find oldest directory (-d) or file (-f)

```bash
find /home/aike -type f -printf '%T+ %p\n' | sort | head -n 1
```

## Find newest file (-d) or file (-f)

```bash
find /home/aike -type f -printf "%T@ %p\n" | sort -n -r | head -n 1
```

## Find by date

```bash
find . -name "*.yaml" -newermt "2023-09-22" ! -newermt "2023-09-23"
```

## FIND INODE USAGE

```bash
find / -xdev -printf '%h\n' | sort | uniq -c | sort -k 1 -n
```
