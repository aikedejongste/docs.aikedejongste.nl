---
layout: default
title: Resource monitoring
has_children: false
parent: Linux
---

# Resource monitoring

## Useful links

* [Percentage of CPU usage](https://stackoverflow.com/questions/26791240/how-to-get-percentage-of-processor-use-with-bash)

## Get cpu stats

```bash
read cpu user nice system idle iowait irq softirq steal guest< /proc/stat
```

## Memory usage

```bash
ps -ax -o comm,rss | grep signal-desktop | awk '{s+=$2} END {print s}'
```

### CPU active
```bash
cpu_active_prev=$((user+system+nice+softirq+steal))
```

## In a script

```
#!/bin/bash

# Read /proc/stat file (for first datapoint)
read cpu user nice system idle iowait irq softirq steal guest< /proc/stat

# compute active and total utilizations
cpu_active_prev=$((user+system+nice+softirq+steal))
cpu_total_prev=$((user+system+nice+softirq+steal+idle+iowait))

usleep 50000

# Read /proc/stat file (for second datapoint)
read cpu user nice system idle iowait irq softirq steal guest< /proc/stat

# compute active and total utilizations
cpu_active_cur=$((user+system+nice+softirq+steal))
cpu_total_cur=$((user+system+nice+softirq+steal+idle+iowait))

# compute CPU utilization (%)
cpu_util=$((100*( cpu_active_cur-cpu_active_prev ) / (cpu_total_cur-cpu_total_prev) ))

printf " Current CPU Utilization : %s\n" "$cpu_util"

exit 0

```






