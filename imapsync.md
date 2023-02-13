---
layout: default
title: Imapsync
has_children: false
---

# Imapsync

## Sync from G-suite to Rackspace

```bash
#!/bin/bash

SOURCEUSER="@option.SourceUsername@"
SOURCEPASS="@option.SourcePassword@"

echo -e "SourceUser is $SOURCEUSER\n"
echo -e "SourcePass is $SOURCEPASS\n"

DESTUSER="@option.DestinationUsername@"
DESTPASS="@option.DestinationPassword@" 

echo -e "DestUser is $DESTUSER\n"
echo -e "DestPass is $DESTPASS\n"

# sync
cd /tmp && imapsync --host1 imap.gmail.com -ssl1 --user1 $SOURCEUSER --password1 $SOURCEPASS \
		    --host2 secure.emailsrvr.com -ssl2 --sslargs2 "SSL_verify_mode=0" --user2 $DESTUSER --password2 $DESTPASS
```

## Install imapsync

[Link for Ubuntu](https://imapsync.lamiral.info/INSTALL.d/INSTALL.Ubuntu.txt)

```bash
  sudo apt-get install  \
libauthen-ntlm-perl     \
libclass-load-perl      \
libcrypt-openssl-rsa-perl \
libcrypt-ssleay-perl    \
libdata-uniqid-perl     \
libdigest-hmac-perl     \
libdist-checkconflicts-perl \
libencode-imaputf7-perl     \
libfile-copy-recursive-perl \
libfile-tail-perl       \
libio-compress-perl     \
libio-socket-inet6-perl \
libio-socket-ssl-perl   \
libio-tee-perl          \
libjson-webtoken-perl   \
libmail-imapclient-perl \
libmodule-scandeps-perl \
libnet-dbus-perl        \
libnet-ssleay-perl      \
libpar-packer-perl      \
libproc-processtable-perl \
libreadonly-perl        \
libregexp-common-perl   \
libsys-meminfo-perl     \
libterm-readkey-perl    \
libtest-fatal-perl      \
libtest-mock-guard-perl \
libtest-mockobject-perl \
libtest-pod-perl        \
libtest-requires-perl   \
libtest-simple-perl     \
libunicode-string-perl  \
liburi-perl             \
libtest-nowarnings-perl \
libtest-deep-perl       \
libtest-warn-perl       \
make                    \
time                    \
cpanminus
```

```bash
sudo wget -N https://raw.githubusercontent.com/imapsync/imapsync/master/imapsync -O /usr/bin/imapsync
sudo chmod 755 /usr/bin/imapsync
```



