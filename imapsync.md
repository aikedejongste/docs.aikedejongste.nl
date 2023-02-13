---
layout: default
title: Imapsync
has_children: false
---

# Sync from G-suite to Rackspace

```bash
echo -e "$SOURCEUSER\n"
echo -e "$SOURCEPASS\n"

echo -e "$DESTUSER\n"
echo -e "$DESTPASS\n"

imapsync \\
    --host1 imap.gmail.com -ssl1 \\
    --user1 $SOURCEUSER --password1 $SOURCEPASS \\
    --host2 secure.emailsrvr.com -ssl2 --sslargs2 "SSL_verify_mode=0" \\
    --user2 $DESTUSER --password2 $DESTPASS
```
