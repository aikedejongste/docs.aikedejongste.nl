---
layout: default
title: Apache WebDav
parent: Webservers
---

# Apache WebDav

## Simple Webdav setup with Auth Digest

### Enable module `auth_digest`:

```bash
a2enmod auth_digest
```
### Create password file

```bash
htdigest -c /etc/apache2/.htdigest "webdav" aike
```

### The vhost config

```
<VirtualHost *:443>
   ServerName grafana.you
   < logging and certs >
   RewriteEngine on

   Alias /files /opt/webdavroot
   <Directory /opt/webdavroot>
     DAV on
     AuthType Digest
     AuthName "webdav"
     AuthDigestDomain /files
     AuthUserFile /etc/apache2/.htdigest
     Require valid-user
   </Directory>

</VirtualHost>
```
