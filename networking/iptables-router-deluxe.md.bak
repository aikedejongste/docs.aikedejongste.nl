---
layout: default
title: IPtables router
parent: Networking
---

# IPtables routing


#First we flush our current rules
iptables -F
iptables -t nat -F

#Setup default policies to handle unmatched traffic
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD DROP

#The interfaces
export LAN=eth0
export WAN=eth1

#Then we lock our services so they only work from the LAN
iptables -I INPUT 1 -i ${LAN} -j ACCEPT
iptables -I INPUT 1 -i lo -j ACCEPT
iptables -A INPUT -p UDP --dport bootps -i ! ${LAN} -j REJECT
iptables -A INPUT -p UDP --dport domain -i ! ${LAN} -j REJECT

#We also accept stuff from our VPN tunnel
iptables -I INPUT 1 -i ${VPN} -j ACCEPT

#(Optional) Allow access to our ssh server from the WAN
iptables -A INPUT -p TCP --dport ssh -i ${WAN} -j ACCEPT

#(Optional) Allow access to our http server from the WAN
#iptables -A INPUT -p TCP --dport http -i ${WAN} -j ACCEPT

#Drop TCP / UDP packets to privileged ports
iptables -A INPUT -p TCP -i ! ${LAN} -d 0/0 --dport 0:1023 -j DROP
iptables -A INPUT -p UDP -i ! ${LAN} -d 0/0 --dport 0:1023 -j DROP

#Finally we add the rules for NAT
iptables -I FORWARD -i ${LAN} -d 192.168.78.0/255.255.255.0 -j DROP
iptables -A FORWARD -i ${LAN} -s 192.168.78.0/255.255.255.0 -j ACCEPT
iptables -A FORWARD -i ${WAN} -d 192.168.78.0/255.255.255.0 -j ACCEPT
iptables -t nat -A POSTROUTING -o ${WAN} -j MASQUERADE

#Tell the kernel that ip forwarding is OK
echo 1 > /proc/sys/net/ipv4/ip_forward
for f in /proc/sys/net/ipv4/conf/*/rp_filter ; do echo 1 > $f ; done

#Port forwards
#g0db0x torrent (voorbeeldje)
#iptables -t nat -A PREROUTING -p tcp --dport 10666 -i ${WAN} -j DNAT --to 192.168.1.156
#iptables -t nat -A PREROUTING -p udp --dport 4444 -i ${WAN} -j DNAT --to 192.168.1.156
#iptables -t nat -A PREROUTING -p udp --dport 4445 -i ${WAN} -j DNAT --to 192.168.1.156

#NeoNova SSH
iptables -t nat -A PREROUTING -p tcp --dport 2222 -i ${WAN} -j DNAT --to 192.168.78.99
iptables -t nat -A PREROUTING -p tcp --dport 80 -i ${WAN} -j DNAT --to 192.168.78.99
iptables -t nat -A PREROUTING -p tcp --dport 443 -i ${WAN} -j DNAT --to 192.168.78.99

#Blocked ports
iptables -A INPUT -p tcp --dport smtp -i ${WAN} -j REJECT
iptables -A INPUT -p tcp --dport 6600 -i ${WAN} -j REJECT
iptables -A INPUT -p tcp --dport nfs -i ${WAN} -j REJECT

#(Optional) Allow access to MUNIN from the WAN
iptables -A INPUT -p TCP --dport 4949 -s 87.253.149.111 -i ${WAN} -j ACCEPT

