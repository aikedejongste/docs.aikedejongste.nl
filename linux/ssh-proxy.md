---
layout: default
title: SSH via proxy
has_children: false
parent: Linux
---

# SSH via Proxy

{: .highlight }
Looks like I have copied this from somewhere....

## Scenario 1: The client has access to a server in a DMZ

The client has access to a server in an internet DMZ, which in turn can access the external server on the internet. Most Linux servers nowadays have Netcat installed, so this fairly trivial constellation works 95.4% of the time.

~/.ssh/config
```
Host host.external
ServerAliveInterval 10
ProxyCommand ssh host.dmz /usr/bin/nc -w 60 host.external 22
```

## Scenario 2: As scenario 1, but the server in the DMZ doesn’t have Netcat

It may not have Netcat, but it surely has an ssh client, which we use to run an instance of sshd in inetd mode on the destination server. This will be our ProxyCommand.

 ~/.ssh/config
```
Host host.external
ServerAliveInterval 10
ProxyCommand ssh -A host.dmz ssh host.external /usr/sbin/sshd -i
```

## Scenario 2½: Modern version of the Netcat scenario (Update)

Since OpenSSH 5.4, the ssh client has it’s own way of reproducing the Netcat behavior from scenario 1:

 ~/.ssh/config
```
Host host.external
ServerAliveInterval 10
ProxyCommand ssh -W host.external:22 host.dmz
```

## Scenario 3: The client has access to a proxy server

The client has access to a proxy server, through which it will connect to an external SSH service running on Port 443 (because no proxy will usually allow connecting to port 22).

```
Host host.external
ServerAliveInterval 10
ProxyCommand /usr/local/bin/corkscrew 
   proxy.server 3128 
   host.external 443 
   ~/.corkscrew/authfile
username:password
```
(Omit the authfile part, if the proxy does not require authentication.)

## Scenario 4: The client has access to a very restrictive proxy server

This proxy server has authentication, knows it all, intercepts SSL sessions and checks for a minimum client version.

```
Host host.external
ServerAliveInterval 10
ProxyCommand /usr/local/bin/proxytunnel 
   -p proxy.server:3128 
   -F ~/.proxytunnel.auth 
   -r host.external:80 
   -d 127.0.0.1:22 
   -H "User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:29.0) Gecko/20100101 Firefox/29.0\nContent-Length: 0\nPragma: no-cache"
```

~/.proxytunnel.auth
```
proxy_user=username
proxy_passwd=password
```

What happens here:

host.external has an apache web server running with forward proxying enabled.
proxytunnel connects to the proxy specified with -r, via the corporate proxy specified with -p and uses it to connect to 127.0.0.1:22, on the forward-proxying apache.
It sends a hand-crafted request header to the intrusive proxy, which mimics the expected client version.
Mind you that although the connection is to a non-SSL service, it still is secure, because encryption is being brought in by SSH.
What we have here is a hand-crafted exploit against the know-it-all proxy’s configuration. Your mileage may vary.

Super sensible discretion regarding the security of your internal network is advised. Don’t fuck up, don’t use this to bring in anything that will spoil the fun. Bypass all teh firewalls responsibly.



