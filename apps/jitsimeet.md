---
layout: default
title: Jitsi Meet
has_children: false
parent: Self Hosted Apps
---

# Jitsi Meet

## Customize styling the stupid way

```
cp favicon.ico /usr/share/jitsi-meet/favicon.ico
cp favicon.ico /usr/share/jitsi-meet/images/favicon.ico
cp jitsilogo.png /usr/share/jitsi-meet/images/jitsilogo.png
cp jitsiLogo_square.png /usr/share/jitsi-meet/images/jitsiLogo_square.png
cp title.htlm /usr/share/jitsi-meet/title.html
cp close2.htlm /usr/share/jitsi-meet/close2.html
cp interface_config.js /usr/share/jitsi-meet/interface_config.js
cp watermark.png /usr/share/jitsi-meet/images/watermark.png
```

and

```
.tile-view .videocontainer#largeVideoContainer { background-color: #ffffff !important; }
.tile-view #largeVideoContainer
```
