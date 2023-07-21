---
layout: default
title: IPtables commands
parent: Networking
---

# IPtables commands

## Order of the rules

In UFW, the order of the rules matters. The rules are evaluated in the order they are created. So if you have a rule that allows traffic to port 80 from anywhere, and it's placed before your deny rule for a specific IP, the allow rule will take precedence.

Unfortunately, UFW does not have a built-in way to reorder existing rules. If you need to change the order of your rules, you have to delete and recreate them in the order you want them to be evaluated.

## Useful links

* https://phoenixnap.com/kb/iptables-port-forwarding#ftoc-heading-8
* https://wiki.nftables.org/wiki-nftables/index.php/Main_Page

## Check syntax

```bash
iptables-restore -t /etc/iptables/rules.v4
```

## Reload rules

```bash
service netfilter-persistent reload
```

or

```bash
iptables-restore < /etc/iptables/rules.v4
```

## Show rules or config

```bash
iptables -L
```

```bash
iptables -S
```

## Clean up

```bash
iptables -F; iptables -t nat -F; iptables -t mangle -F
```

## Route/NAT/Masquerade

Just this should do it. But it is insecure.

```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

## SNAT and Masquerade between subnets

If eth1 is 192.168.1.2.

```
iptables -t nat -A POSTROUTING -o eth1 -s 10.0.0.0/24  -j MASQUERADE
```
==
```
iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o eth1 -j SNAT --to-source 192.168.1.2
```


## Connection tracking

Connection tracking stores connections in a table, which allows administrators to allow or deny access based on the following connection states:
* NEW — A packet requesting a new connection, such as an HTTP request.
* ESTABLISHED — A packet that is part of an existing connection.
* RELATED — A packet that is requesting a new connection but is part of an existing connection, such as passive FTP connections where the connection port is 20, but the transfer port can be any unused port 1024 or higher.
* INVALID — A packet that is not part of any connections in the connection tracking table.

```bash
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
```

## Add default route

```bash
ip route add 0.0.0.0/0 via 10.118.0.2
```

## IPtables NAT:

```bash
# DST is usually internal IP in local lan
DST=10.1.2.3
# SRC is usually public IP on the internet
SRC=60.70.80.90
# Port
PRT=9000
/sbin/iptables -A FORWARD -p tcp -d $DST --dport $PRT -s $SRC -j ACCEPT
/sbin/iptables -A PREROUTING -t nat -i ens3 -p tcp --dport $PRT -j DNAT --to $DST:$PRT
```

## Bock all traffic from 1 ip address

```bash
sudo iptables -A INPUT -s 1.2.3.4 -j DROP
```
