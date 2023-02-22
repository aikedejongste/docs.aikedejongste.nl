---
layout: default
title: Mail
has_children: false
---

# Mail, SPF, DMARC, DKIM, etc

## Setup

1. Check the current state: https://toolbox.googleapps.com/apps/checkmx/


## Theory

Link: https://www.courier.com/guides/dmarc-vs-spf-vs-dkim/

* SPF stands for Sender Policy Framework. It allows you to cache a list of authorized IP addresses that are allowed to send emails to your customers on your behalf.

* DKIM allows a sender to attach DKIM signatures to email headers and validate them using a public cryptographic key found in the company's DNS record. 

* DMARC requires you to first configure the SPF and DKIM protocols. And can then be used to decide how to handle unauthenticated messages.

### DMARC

* Policy = (p=none): no action is taken, and the message is delivered as usual.
* Policy = (p=quarantine): sends the message to the spam/junk/quarantine folder
* Policy = (p=reject): sends the message back

