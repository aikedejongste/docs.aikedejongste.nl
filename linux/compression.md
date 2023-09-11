---
layout: default
title: Gzip / pigz
has_children: false
parent: Linux
---

# Compression

## pigz

### Install

```bash
apt install pigz
```

### Compress single file:

```bash
pigz filename.txt
```

### Compress directory:

```bash
tar --use-compress-program="pigz -k " -cf oldsystem.tgz OLDSYSTEM/
```

### Decompress single file:

```bash
pigz -dc data.pigz > data.out
```

### Decompress directory:



## gzip

```bash
tar -cvzf my_directory.tgz my_directory
```
