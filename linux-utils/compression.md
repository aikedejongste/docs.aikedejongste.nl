---
layout: default
title: Gzip / pigz
has_children: false
parent: Linux
---

# Compression

## rar

```bash
apt install unrar-free
```


## zip

Useful when compressing for Windows. Might need 7zip on Windows to
extract password protected archives.

Do not use `-p`, use `-e` instead.

```bash
zip -r -e -P password archive-name.zip dir-to-be-compressed/
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

```bash
tar -I pigz -xvf compressed_file.tar.gz
```

## gzip

```bash
tar -cvzf my_directory.tgz my_directory
```

## Benchmarks I found on the internet

| Compression| 	Size | 	Time Elapsed|	Command|
| gzip	| 954 MB|	2:10|	tar cf - AOM/ \| gzip -9 - > AOM.tar.gz|
| xz	| 847 MB|	27:32|	tar cf - AOM/ \| xz -9e - > AOM.tar.xz|
| bzip2	| 943 MB|	5:42|	tar cf - AOM/ \| bzip2 -9 - > AOM.tar.bz2|
| 7zip	| 845 MB|	16:41|	7z a -mx=9 AOM.7z AOM/|
| zip	| 955 MB|	2:05|	zip -9 -r AOM.zip AOM/|
| rar	| 876 MB|	6:31|	rar a -m5 AOM.rar AOM/*|
| zstd	| 873 MB|	22:19|	tar -I 'zstd --ultra -22' -cf AOM.tar.zst AOM/ |
