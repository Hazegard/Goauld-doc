---
title: Install
weight: 1
---

We currently do not provide prebuilt binaries.

See [general/compilation]({{< ref "01-general/02-compilation" >}}) to compile the components.

## Server

See [server/deployment]({{< ref "03-server/02-deployment" >}}) to install and deploy the server.

## Client


```yaml
# Access token required to access the /manage/ endpoint.
access-token: XXXXXXXXXX

# HTTP Server to connect to.
server: https://example.com

# SSH Server to connect to.
ssh-server: example.com:22222
```