---
layout: default
title: Apache
parent: Webservers
---

# Apache2

## Simple proxy pass with websockets

```
<VirtualHost *:443>
  ServerName grafana.you
  ServerAdmin webmaster@localhost
  ErrorLog ${APACHE_LOG_DIR}/grafana.you-error.log
  CustomLog ${APACHE_LOG_DIR}/grafana.you-access.log combined

  SSLEngine on
  SSLCertificateFile      /etc/ssl/certs/star.you.app.pem
  SSLCertificateKeyFile   /etc/ssl/private/star.you.app.key

  RewriteEngine on

  # Private
  <Location />
    <RequireAll>
      <RequireAny>
        # VPN
        Require ip 1.2.3.4
      </RequireAny>
    </RequireAll>

    RewriteCond %{HTTP:Upgrade} websocket [NC]
    RewriteCond %{HTTP:Connection} upgrade [NC]
    RewriteRule ^/?(.*) "ws://127.0.0.1:3000/$1" [P,L]
    ProxyPreservehost On
    ProxyPass        http://127.0.0.1:3000/
    ProxyPassReverse http://127.0.0.1:3000/
  </Location>

  # Public
  <Location /login/github>
    Require all granted

    ProxyPreservehost On
    ProxyPass        http://127.0.0.1:3000/
    ProxyPassReverse http://127.0.0.1:3000/
  </Location>

</VirtualHost>
```

## Make one subdir public

```
  <Location "/">
    Require ip ${ip_addr_vpn}

    ProxyPass          balancer://internal-ip/
    ProxyPassReverse   balancer://internal-ip/
  </Location>

  <Location "/api">
    Require all granted

    ProxyPass          balancer://internal-ip/api
    ProxyPassReverse   balancer://internal-ip/api
  </Location>
```

## Make one subdir private

```
    <Location /admin>
        Require ip 1.2.3.4
    </Location>

   ProxyPreserveHost On
   ProxyPass        / http://internal-ip:80/
   ProxyPassReverse / http://internal-ip:80/
```

## ProxyPass to invalid certificate

```
    SSLProxyEngine on
    SSLProxyVerify none
    SSLProxyCheckPeerCN off
    SSLProxyCheckPeerName off
```

## Use variables for IP adresses

In Apache config somewhere:

```
Define ip_addr_vpn 1.2.3.4
```

In vhost use:

```
  Require ip ${ip_addr_vpn}
```

## Avoid ProxyPass

Use this to avoid ProxyPasses catching it

`ProxyPass /server-status !`

## Server-Status NOT in VHOST but in mods-enabled/status.conf

There is a default status location block, just add your IP addresses.

```conf
<Location "/server-status">
    SetHandler server-status
    Require ip 127.0.0.1 your_ip_address_or_network
</Location>
```

## Related

```conf
<Location "/md-status">
  SetHandler md-status
  Require ip 127.0.0.1 1.2.3.4
</Location>
```

## Errors

### AH03307: ap_proxy_transfer_between_connections: error on sock - ap_pass_brigade
