---
layout: default
title: OpenVPN
has_children: false
parent: Networking
---

# OpenVPN

## Renew OpenVPN server certificate

You need to replace only server.crt and server.key files. No change needed at clients side.

I've found this [here](https://forums.openvpn.net/viewtopic.php?t=44009).

### 1. delete old server.crt key and all files related to it

```bash
in my case (debian linux):
rm /etc/easy-rsa/pki/private/server.key
rm /etc/easy-rsa/pki/issued/server.crt
rm /etc/easy-rsa/pki/private/server.key
rm /etc/easy-rsa/pki/reqs/server.req (maybe you will not have this file so ignore and continue)
```

### 2. generate new certificate named server.crt

Go to your easyrsa folder (in my case cd /etc/openvpn/easy-rsa) and run

```bash
./easyrsa build-server-full server nopass
```

### 3. find your new generated certifiacte in

* easy-rsa/pki/issued folder and validate that you have new server.crt by file creation date.
* easy-rsa/pki/private folder and validate that you have new server.key by file creation date.

### 4. Ensure that server.crt expire date is plus 2 years from now. run

```bash
openssl x509 -in /etc/easy-rsa/pki/issued/server.crt -text -noout | grep "Not After"
```

### 5. Copy new server.crt and server.key to openvpn server folders

```bash
cp easy-rsa/pki/issued/server.crt /etc/openvpn/server/issued
cp easy-rsa/pki/privateserver.key /etc/openvpn/server/private
```

### 6. Now you must restart openvpn service

```bash
systemctl restart openvpn
```
