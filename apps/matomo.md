---
layout: default
title: Matomo
has_children: false
parent: Self Hosted Apps
---

# Matomo

## Update config.ini.php when running on k8s

This seems hacky to me, but couldn't find a better way with the Helm chart. [example on kubectl page](https://docs.aikedejongste.nl/kubectl.html#copy-file-from-local-to-pod)

## Protect login page from the internet

For example when you use Apache as a reverse proxy. Or adjust to use it directly.

The reason for doing this is that you want matomo.php and .js public for tracking, but not the admin pages.

The location block for / comes first, and then the more detailed ones.

```
<Location />
  <RequireAll>
    <RequireAny>
      Require ip 1.2.3.4
      Require ip ${or-use-a-variable}
    </RequireAny>
  </RequireAll>

  ProxyPreserveHost On
  ProxyPass         balancer://to-the-real-server/
</Location>

<Location /matomo.js>
  <RequireAll>
    Require all granted
  </RequireAll>
  ProxyPreserveHost On
  ProxyPass         balancer://to-the-real-server/matomo.js
</Location>

<Location /matomo.php>
  <RequireAll>
    Require all granted
  </RequireAll>
  ProxyPreserveHost On
  ProxyPass         balancer://to-the-real-server/matomo.php
</Location>
```


Next time try a LocationMatch block:

```
<LocationMatch "^/(matomo.js|matomo.php)">
  <RequireAll>
    Require all granted
  </RequireAll>
  ProxyPreserveHost On
  ProxyPass         balancer://to-the-real-server/here-is-the-problem.phpjs
</LocationMatch>
```

