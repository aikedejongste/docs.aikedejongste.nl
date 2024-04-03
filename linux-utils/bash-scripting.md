---
layout: default
title: Bash Scripting
has_children: false
parent: Linux
---

# Bash

## Check if variable is set and not empty

Usually best way?

```bash
if [ -v $MYVAR  ]; then
  not set
else
  echo "is set to $MYVAR"
fi
```

Other ways:

```bash
# -z operator that checks if the variable is empty.
if [ -z "${myvar}" ]; then

# -z operator that checks if the variable is
if [ -z "${myvar+x}" ]; then

#
if [ -z "${myvar+set}" ]; then

#
if [ -z "${myvar-unset}" ]; then

# -n operator that checks if the variable is not empty. So opposite of -z.
if [ -n "$myvar" ]; then

# -v operator that checks the variable’s existence regardless of whether the variable is empty or not.
# only in newer versions of bash
if [ -v "$myvar" ]; then

# using isset function
isset() {
    [ -n "${!1}" ]
}
myvar="Hello"
if isset myvar; then

```

## Ask for yes or no

```bash

< some commands >

while true; do
    read -p "Do you wish to delete this directory? [Y/n] " yn
    case $yn in
        [Yy]* ) break;;  # if 'y' or 'Y' is entered, break the loop and continue with the rest of the script
        [Nn]* ) exit;;   # if 'n' or 'N' is entered, exit the script immediately
        * ) echo "Please answer yes or no.";;  # if anything else is entered, ask again
    esac
done

< more commands >
```

## Bash script template

```bash
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

## Another template

```bash
#!/usr/bin/env bash

set -o errexit
set -o nounset
set -o pipefail
if [[ "${TRACE-0}" == "1" ]]; then
    set -o xtrace
fi

if [[ "${1-}" =~ ^-*h(elp)?$ ]]; then
    echo 'Usage: ./script.sh arg-one arg-two

This is an awesome bash script to make your life better.

'
    exit
fi

cd "$(dirname "$0")"

main() {
    echo do awesome stuff
}

main "$@"
```
