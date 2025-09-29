---
layout: default
title: SOP - renew SSL cert
parent: SOPs
---

# Renew SSL cert

{: .highlight }
Considering not using manually created certs first!

## External checks

- [ ] is the SSL cert at least a B+ [SSLlabs](https://ssllabs.com/ssltest/analyze.html)
- [ ] is uptime monitoring in place and assigned to the right person

## Configuration checks

- [ ] no secrets in container

## Request a cert

### Create a key

```bash
aike@yourcompany-dev ~/repos/certs-and-keys $ openssl ecparam -out star-yourcompany-app.key -name prime256v1 -genkey

aike@yourcompany-dev ~/repos/certs-and-keys $ ls -lash
4.0K -rw-------  1 aike aike  302 Oct  5  2021 star-yourcompany-app.key
```

### Create CSR

```bash
aike@yourcompany-dev ~/repos/certs-and-keys $ openssl req -new -key star-yourcompany-app.key -out star-yourcompany-app.csr

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----

Country Name (2 letter code) [AU]:NL
State or Province Name (full name) [Some-State]:Zeeland
Locality Name (eg, city) []:Zierikzee
Organization Name (eg, company) [Internet Widgits Pty Ltd]: Yourcompany BV
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:*.yourcompany.app
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request

A challenge password []: < empty >
An optional company name []: < empty >

aike@yourcompany-dev ~/repos/certs-and-keys $ ls -lash
4.0K -rw-rw-r--  1 aike aike  485 Oct  5  2021 star-yourcompany-app.csr
4.0K -rw-------  1 aike aike  302 Oct  5  2021 star-yourcompany-app.key
```

## Upload the CSR to the Cert provider
