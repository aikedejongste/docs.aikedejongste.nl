---
layout: default
title: Knockd
parent: Networking
---

# Knockd

If 1.2.3.4 is the IP of the hidden server:

```
root@hidden-box:~# cat /etc/knockd.conf 
[options]
  UseSyslog

[sql_open]
  sequence = 1234,4567,8901,2345
  tcpflags = syn
  seq_timeout = 10
  start_command = iptables -A FORWARD -s %IP% -d 1.2.3.4/32 -p tcp -m tcp --dport 22 -j ACCEPT
  cmd_timeout = 86400
  stop_command = iptables -D FORWARD -s %IP% -d 1.2.3.4./32 -p tcp -m tcp --dport 22 -j ACCEPT
```

