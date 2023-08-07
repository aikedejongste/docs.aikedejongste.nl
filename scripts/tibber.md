---
layout: default
title: Tibber
parent: Various scripts
has_children: false
---

# Tibber

## Simple script that uses Folding@Home to burn electricity

```bash
#!/bin/bash

# Function to parse price and check if it's negative
check_price () {
    PRICE=$(curl -s -H "Authorization: Bearer $TIBBER_API_KEY" -H "Content-Type: application/json" -d '{
        "query": "{ viewer { homes { currentSubscription{ priceInfo{ current{ total }}}}}}"
    }' https://api.tibber.com/v1-beta/gql | jq -r '.data.viewer.homes[0].currentSubscription.priceInfo.current.total')

    if (( $(echo "$PRICE < 0" |bc -l) )); then
        return 0
    else
        return 1
    fi
}

# Start your task if the price is negative
# Still trying to find the best way to use multiple cores
if check_price; then
    echo "Price is $PRICE. That is negative. Starting the task!!!"
    docker run --pull --rm -d \
               --name=foldingathome \
               -p 7396:7396 \
               -e POWER=full \
               -e ENABLE_SMP=true \
               -v ~/fah:/config \
               lscr.io/linuxserver/foldingathome:latest
else
    echo "Price is $PRICE. That is positive. Stopping task if running."
    docker stop foldingathome; docker rm foldingathome
fi
```

## Crontab setup

```
TIBBER_API_KEY=YVAEVQ..............

# m h  dom mon dow   command
0 * * * * /root/burn.sh
```

## Requirements

```bash
apt install docker.io jq bc
```
