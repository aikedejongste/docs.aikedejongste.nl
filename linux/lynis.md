---
layout: default
title: Lynis
has_children: false
parent: Linux
---

# Lynis

## Installation on Ubuntu

```bash
sudo apt install dirmngr apt-transport-https
```

```bash
sudo apt-key adv --keyserver [keyserver.ubuntu.com](http://keyserver.ubuntu.com/) \
		 --recv-keys 013baa07180c50a7101097ef9de922f1c2fde6c4
```

```bash
echo 'Acquire::Languages "none";' | sudo tee /etc/apt/apt.conf.d/99disable-translations
echo "deb [https://packages.cisofy.com/community/lynis/deb/](https://packages.cisofy.com/community/lynis/deb/) stable main" | sudo tee /etc/apt/sources.list.d/cisofy-lynis.list
```

```bash
sudo apt update && sudo apt install -y lynis
```
