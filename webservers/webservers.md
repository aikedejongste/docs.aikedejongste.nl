---
layout: default
title: Webservers
has_children: true
---

# Webservers

## Check security headers

1. [Mozilla Observatory](https://observatory.mozilla.org/)
2. [SecurityHeaders.com](https://securityheaders.com/)

## HTTP2

1. HTTP/2 is a binary protocol, as opposed to HTTP 1.1 that is plain text. The latter is meant to be human readable (for example sniffing network traffic) meanwhile the former is not.
2. h2 is HTTP/2 over TLS (protocol negotiation via ALPN).
3. h2c is HTTP/2 over TCP.
4. A frame is the smallest unit of communication within an HTTP/2 connection, consisting of a header and a variable-length sequence of octets structured according to the frame type.
5. A stream is a bidirectional flow of frames within the HTTP/2 connection. The correspondent concept in HTTP 1.1 is a request/response message exchange.
6. HTTP/2 is able to run multiple streams of data over the same TCP connection, avoiding the classic HTTP 1.1 head of blocking slow request and avoiding to re-instantiate TCP connections for each request/response (KeepAlive patched the problem in HTTP 1.1 but did not fully solve it).
