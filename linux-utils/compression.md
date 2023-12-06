---
layout: default
title: Gzip / pigz
has_children: false
parent: Linux
---

# Compression

## zip

Useful when compressing for Windows. Might need 7zip on Windows to
extract password protected archives.

Do not use `-p`, use `-e` instead.

```bash
zip -r -e archive-name.zip dir-to-be-compressed/
```

## pigz

### Install

```bash
apt install pigz
```

### Compress single file and delete original

```bash
pigz filename.txt
```

### Compress single file and keep the original

```bash
pigz -k filename.txt
```

### Pigz check file contents

```bash
pigz -l file.gz
```

### Compress directory

```bash
tar --use-compress-program="pigz -k " -cf oldsystem.tgz OLDSYSTEM/
```

### Decompress single file

```bash
pigz -dc data.pigz > data.out
```

### Decompress directory

## gzip

```bash
tar -cvzf my_directory.tgz my_directory
```
