---
title: Install
weight: 1
---

Prebuilt client and server are available at [https://github.com/Hazegard/Goauld/releases](https://github.com/Hazegard/Goauld/releases).

See [general/compilation]({{< ref "01-general/02-compilation" >}}) to compile the components.

## Generate age key

Install [age](https://github.com/filosottile/age), then

```bash
$ age-keygen
# created: 2026-03-27T21:44:18+01:00
# public key: age1krjxdnhmf2kqm8rdhyf6sr5nfvlwdcslux3fxt8amcrncwn3ss9sydlvd0
AGE-SECRET-KEY-1NJ4DRPNKNGEVFK50JHUD6RKZ3NJ3Q9S5KYMTARTLXU0P0KQU8AAQNE4C2F
```

The private key (`AGE-SECRET-KEY-*`) will be used in the server, and the public key in the agents.

## Generate access token (and admin token)

```bash
openssl rand -base64 42
```

These tokens will be used in both server and clients.

## Server

See [server/deployment]({{< ref "03-server/02-deployment" >}}) to install and deploy the server.

## Client

A minimal configuration file is:

```yaml
# Access token required to access the /manage/ endpoint.
access-token: XXXXXXXXXX

# HTTP Server to connect to.
server: https://example.com

# SSH Server to connect to.
ssh-server: example.com:22222
```