---
layout: default
title: Mail
has_children: true
---

# Mail, SPF, DMARC, DKIM, etc

## Setup

1. Check the current state of MX records: [Google Check](https://toolbox.googleapps.com/apps/checkmx/).

## Theory

Link: <https://www.courier.com/guides/dmarc-vs-spf-vs-dkim/>

* SPF stands for Sender Policy Framework. It allows you to cache a list of authorized IP addresses that are allowed to send emails to your customers on your behalf.

* DKIM allows a sender to attach DKIM signatures to email headers and validate them using a public cryptographic key found in the company's DNS record.

* DMARC requires you to first configure the SPF and DKIM protocols. And can then be used to decide how to handle unauthenticated messages.

* ARC checks the previous authentication status of forwarded messages. If a forwarded message passes SPF or DKIM authentication, but ARC shows it previously failed authentication, mailservers treat the message as unauthenticated.

### DMARC

* Policy = (p=none): no action is taken, and the message is delivered as usual.
* Policy = (p=quarantine): sends the message to the spam/junk/quarantine folder
* Policy = (p=reject): sends the message back

Check your DMARC record [here](https://mxtoolbox.com/SuperTool.aspx?action=dmarc).

Example:

`_dmarc.aike.nl TXT v=DMARC1; p=none;"

### SPF

Decide how you want to enforce SPF failures:

```
    ~all: results in a soft fail (Not authorized, but not explicitly unauthorized).
    -all: results in a hard fail (Unauthorized).
    ?all: neutral (As if there is no policy at all).
```

Check your SPF record [here](https://mxtoolbox.com/SuperTool.aspx?action=spf).

## MTA-STS

* [info here](https://powerdmarc.com/what-is-mta-sts-and-why-do-you-need-it/)

MTA-STS protocol is deployed by having a DNS record that specifies that a mail
server can fetch a policy file from a specific subdomain. This policy file is
fetched via HTTPS and authenticated with certificates, along with the list of
names of the recipient’s mail servers. Implementing MTA-STS is easier on the
recipient’s side in comparison to the sending side as it requires to be
supported by the mail server software. While some mail servers support MTA-STS,
such as PostFix, not all do.

## Send with Telnet

```bash
telnet <HOST> 25

EHLO aike.com
MAIL FROM:<test@source.com>
RCPT TO:<aap@destination.com>
DATA
Subject: Test1


Body verhaaltje

.
```
