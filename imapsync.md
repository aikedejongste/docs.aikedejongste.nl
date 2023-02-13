---
layout: default
title: Imapsync
has_children: false
---

# Sync from G-suite to Rackspace

```bash
imapsync
    --host1 imap.gmail.com -ssl1 \\
    --user1 @option.DestinationUsername@ --password1 @option.DestinationPassword@ \\
    --host2 secure.emailsrvr.com -ssl2 --sslargs2 "SSL_verify_mode=0" \\
    --user2 @option.SourceUsername@ --password1 @option.SourcePassword@
```
