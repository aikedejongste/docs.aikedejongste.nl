---
layout: default
title: Vim
parent: Editors
has_children: false
---

# Vim

## Server basic config

```bash
echo "set ts=2 sts=2 sw=2 et ai si number cursorline showcmd incsearch ignorecase" >> ~/.vimrc
```

## Write with sudo when readonly

```bash
:w !sudo tee %
```

## Indent 4 spaces

```bash
    :%normal! I
```

Here's a step-by-step breakdown:
 • :%: Select all lines.
 • normal!: Execute the following command in normal mode.
 • I : Insert 4 spaces at the beginning of each line.
 • Esc: Return to normal mode after insertion.
 • Save the file and exit by typing :wq.
