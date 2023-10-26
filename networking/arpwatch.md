---
layout: default
title: Arpwatch
parent: Networking
---

# Arpwatch Ubuntu

```bash
apt install arpwatch
systemctl enable arpwatch@eno1 & systemctl start arpwatch@eno1
journalctl -uarpwatch@eno1 -f
```
