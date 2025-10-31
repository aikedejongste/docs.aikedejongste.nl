---
layout: default
title: Backup buckets
has_children: false
parent: Scripts
---

## Sync buckets

```bash
#!/bin/bash

export MONITOR_URL='https://cronitor.link/p/......'
curl $MONITOR_URL?state=run

# Turn on debugging output
set -x

# Function to execute at the end
cleanup() {
  sleep 15m && sudo poweroff
}

# Register cleanup function to be called on script exit
trap cleanup EXIT

# Initial Sleep
echo "Sleep to wait for network"
sleep 30

# Log Date
date >> /home/aike/log.log

# Destination path
DEST_PATH="/home/aike/b2-backup"

# Get list of all buckets
BUCKETS=$(rclone lsd b2: | awk '{print $5}') || { echo "Failed to list buckets"; exit 1; }

# Loop through each bucket and sync
for BUCKET in $BUCKETS; do
    echo "Syncing bucket $BUCKET..."
    rclone copy "b2:$BUCKET/" "$DEST_PATH/$BUCKET" || { echo "Failed to sync bucket $BUCKET"; exit 1; }
done

# Notify monitor of completion
curl "$MONITOR_URL?state=complete&msg=Success!"

# Disk usage
du -hs $DEST_PATH/* > usage.txt
```
