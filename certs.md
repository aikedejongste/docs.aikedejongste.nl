---
layout: default
title: Certificates
has_children: false
---

# Certificates (SSL/TSL)

## Favorite vendor

[SSLs.com](https://ssls.sjv.io/vNzVeW)

## Favorite generator

[mkcert](https://github.com/FiloSottile/mkcert)

## Notes

- PKCS#7 does not include the private (key) part of a certificate/private-key pair.
- PKCS#12 is a more universal container - it is intended to store both the private key and public certificate parts together.
- PFX was the predecessor of PKCS#12.

## Check Letsencrypt/Certbot TXT record

Check if the txt record has been processed:

```bash
host -t txt _acme-challenge.example.com
```

## Self signed cert on Windows

```bash
$cert = New-SelfSignedCertificate -DnsName "dev-feuerfest.app" -CertStoreLocation "cert:\LocalMachine\My"
$pwd = ConvertTo-SecureString -String "wachtwoord" -Force -AsPlainText
Export-PfxCertificate -Cert "cert:\LocalMachine\My\$($cert.Thumbprint)" -FilePath "certificate.pfx" -Password $pwd
```

Add it to Windows key store [link](https://community.spiceworks.com/how_to/1839-installing-self-signed-ca-certificate-in-windows).

## Cloudflare PKI and TLS toolkit

[CloudFlare cfssl](https://github.com/cloudflare/cfssl)

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

If you order from Xolphin then `Apache-Nginx/star_stekker_app-fullchain.txt` is the cert + the chain in the
correct order.

Otherwise the order is like this:

1. cert.pem is cert first then ca bundle
2. cert.key is just the key

or

1. /etc/ssl/certs/star.${fqdn}.crt, paste in the crt, followed by the included CA bundle, include a newline!
2. /etc/ssl/private/star.${fqdn}.key, paste in the key

or

1. Altogether is cert, chain, key

## Full chain for Haproxy:

```bash
export domain=yourdomain.nl
cat /etc/letsencrypt/live/$domain/fullchain.pem /etc/letsencrypt/live/$domain/privkey.pem > fullchainkey.pem

cat /etc/letsencrypt/live/$domain/fullchain.pem /etc/letsencrypt/live/$domain/privkey.pem | base64 -w 0 > fullchainkey.pem
```


## Check k3s cert expiration date

```bash
openssl s_client -connect k3s.company.com:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate
```

## Check config at SSL Labs

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

```bash
openssl x509 -in company.com.pem -text

OR

openssl x509 -in company.com.pem -text | grep Issuer
```

To see if this is a CA inspect the Issuer CN.

## See if .key and .pem/.crt match

The 2 mdsums should match:

```bash
openssl rsa -modulus -noout -in ../private/star.you.nl.key | openssl md5
openssl x509 -modulus -noout -in star.you.nl.pem | openssl md5
```

And this should give "RSA Key is ok".

```bash
openssl rsa -check -noout -in ../private/star.you.nl.key
```

## TransIP Wildcard

Put the private key as one line in the .env file.

```bash
#!/bin/bash -e
docker run -ti \
        --env-file=.env \
        --mount type=bind,source="${PWD}"/ssl,target="/etc/letsencrypt" \
        --mount type=bind,source="${PWD}"/ssl/config,target="/opt/certbot-dns-transip/config" \
        --mount type=bind,source="${PWD}"/ssl/logs,target="/opt/certbot-dns-transip/logs" \
        rbongers/certbot-dns-transip \
        certonly --manual --preferred-challenge=dns  \
        --manual-auth-hook=/opt/certbot-dns-transip/auth-hook \
        --manual-cleanup-hook=/opt/certbot-dns-transip/cleanup-hook \
        -d '*.you.nl'
```

## TransIP Wildcard renew

```bash
#!/bin/bash -e
cd /opt && /usr/bin/docker run -t \
        --env-file=/opt/.env \
        --mount type=bind,source="${PWD}"/ssl,target="/etc/letsencrypt" \
        --mount type=bind,source="${PWD}"/ssl/config,target="/opt/certbot-dns-transip/config" \
        --mount type=bind,source="${PWD}"/ssl/logs,target="/opt/certbot-dns-transip/logs" \
        rbongers/certbot-dns-transip \
        renew
```

## Generate self-signed cert without user input

`openssl req -x509 -newkey rsa:4096 -keyout /etc/ssl/key.pem -out cert.pem -days 365 -nodes -subj '/CN=localhost'`
