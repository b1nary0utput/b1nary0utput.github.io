---
title: "Check Your Public IP Address from the Linux Command Line"
date: 2026-08-14

summary: "Quick reference for checking your public IP address directly from the Linux command line."

description: "Learn how to check your public IPv4 address from the Linux command line using common command-line tools."

slug: "check-public-ip-linux"

tags:
  - Linux
  - Networking
  - Public IP
  - Command Line

categories:
  - Notes

ShowToc: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowCodeCopyButtons: true
---

When working from the Linux command line, you can quickly determine your public IP address by querying an external service.

The simplest method is to use `curl` with a public IP address service:

```bash
curl ifconfig.me
```

or with the following:

```bash
curl -4 icanhazip.com
```

These commands query an external service to determine the public IP address visible from the Internet.
