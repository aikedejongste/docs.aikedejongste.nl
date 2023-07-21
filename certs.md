---
layout: default
title: Certificates
has_children: false
---

# Certificates (SSL/TSL)

## Favorite vendor:

[SSLs.com](https://ssls.sjv.io/vNzVeW)


## Wildcard cert with Letsencrypt and DNS challenge

0. Install python and pip: `apt install python3-pip`
1. Install an authenticator, for example: `pip install certbot-dns-hetzner`
2. Put `dns_hetzner_api_token = I49N1....` in `hetzner.sops.ini`
3. Run:
```bash
chmod 600 hetzner.sops.ini
certbot certonly --no-eff-email --agree-tos -m 'certbot@yourdomain.nl' \
                 --authenticator dns-hetzner --dns-hetzner-credentials hetzner.sops.ini \
                 -d '*.yourdomain.nl' --work-dir . \
                 --logs-dir .
```

## Cert and bundle order

1. cert.pem is cert first then ca bundle
2. cert.key is just the key

or

1. /etc/ssl/certs/star.${fqdn}.crt, paste in the crt, followed by the included CA bundle, include a newline!
2. /etc/ssl/private/star.${fqdn}.key, paste in the key

or

1. alltogether is ....

## Check k3s cert expiration date

```bash
openssl s_client -connect k3s.company.com:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate
```

## Check config at SSL Labs:

[SSL labs](https://ssllabs.com/ssltest/analyze.html)

## Base64 enc for k8s secret

`-w 0` will prevent base64 from adding newlines

```bash
cat star.company.com.crt | base64 -w 0 > star.company.com.crt.enc
cat star.company.com.key | base64 -w 0 > star.company.com.key.enc
```

## Check for new expire date

### Online

```bash
export SITE_URL="company.com"
export SITE_SSL_PORT="443"
openssl s_client -connect ${SITE_URL}:${SITE_SSL_PORT} \
  -servername ${SITE_URL} 2> /dev/null |  openssl x509 -noout  -dates
```

### File

```bash openssl x509 -enddate -noout -in *.crt```

## Export private key from pfx

```bash
openssl pkcs12 -in filename.pfx -nocerts -nodes -out key.pem
```

## Export certificate from pfx


```bash
openssl pkcs12 -in filename.pfx -clcerts -nokeys -out cert.pem
```


## See PEM file contents

```bah
openssl x509 -in company.com.pem -text

OR

openssl x509 -in company.com.pem -text | grep Issuer
```

To see if this is a CA inspect the Issuer CN.
