---
layout: default
title: Grep/awk/sed
parent: Linux
has_children: false
---

# Grep, awk and sed

## Grep for 2 things

```bash
grep 'pattern1\|pattern2' fileName_or_filePath
```

## Filter out comments and blank lines

```bash
grep -Ev "^#|^$" filename
```

## Display lines before or after result

```bash
grep -A 5 'pattern' filename # Display 5 lines after the result
grep -B 5 'pattern' filename # Display 5 lines before the result
grep -C 5 'pattern' filename # Display 5 lines before and after the result
```

