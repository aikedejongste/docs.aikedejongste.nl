---
layout: default
title: Bash Scripting
has_children: false
parent: Linux
---

# Bash

## Bash script template

```
!/bin/bash

lockfile=/tmp/random.lock

if ( set -o noclobber; echo "$$" > "$lockfile") 2> /dev/null; then

    trap 'rm -f "$lockfile"; exit $?' INT TERM EXIT

    # do stuff here
    cd /code/script && time nice -n 10 node bin/run-this.sh
    exit=$?
    # clean up after yourself, and release your trap
    rm -f "$lockfile"
    trap - INT TERM EXIT

else echo "Lock Exists: $lockfile owned by $(cat $lockfile)" exit=1 fi

exit $exit
```


