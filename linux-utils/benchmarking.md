---
layout: default
title: Benchmarking
has_children: false
parent: Linux
---

# Benchmarking

## Disk with dd

```bash
dd bs=1M count=256 if=/dev/zero of=deleteme conv=fdatasync
```

## Disk with hdparm

Some results here: [gist](https://gist.github.com/AikedeJongste/f1e4fbf0ad532833a81878e6af995b02)

```bash
hdparm -Tt /dev/vda
```

## Disk with fio

### Sequential writes with 1Mb block size. Imitates write backup activity or large file copies

```bash
fio --name=fiotest --filename=test1 --size=16GB --rw=write --bs=1M --direct=1 --numjobs=8 --ioengine=libaio --iodepth=8 --group_reporting --runtime=60 --startdelay=60
rm test1
```

### Sequential reads with 1Mb block size. Imitates read backup activity or large file copies

```bash
fio --name=fiotest --filename=test1 --size=16GB --rw=read --bs=1M --direct=1 --numjobs=8 --ioengine=libaio --iodepth=8 --group_reporting --runtime=60 --startdelay=60
rm test1
```

### Test read IOPS by performing random reads, using an I/O block size of 4 KB and an I/O depth of at least 256:

```bash
TEST_DIR=/mnt/fiotest
mkdir -p $TEST_DIR

fio --name=read_iops --directory=$TEST_DIR --size=10G \
--time_based --runtime=60s --ramp_time=2s --ioengine=libaio --direct=1 \
--verify=0 --bs=4K --iodepth=256 --rw=randread --group_reporting=1 \
--iodepth_batch_submit=256  --iodepth_batch_complete_max=256
```

|  Device | Result |
|:-------------|:------------------|
| X1 Carbon 10th gen | read: IOPS=357k, BW=1395MiB/s (1463MB/s)(81.7GiB/60001msec) |
| Unknown Azure storage disk | read: IOPS=8097, BW=31.6MiB/s (33.2MB/s)(1899MiB/60007msec) |
| Unknown Azure boot disk | read: IOPS=4225, BW=16.5MiB/s (17.3MB/s)(993MiB/60103msec) |
| Hetzner VPS | read: IOPS=104k, BW=406MiB/s (425MB/s)(23.8GiB/60003msec) |
