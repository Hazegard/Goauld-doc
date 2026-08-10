---
title: Compile agent
description:
weight: 12
---

The client can recompile agents for different platforms and architectures.


```bash
tealc compile (--id [ID]) (-O [OS]) (-A [ARCH])
```

The `ARCH` flag can be:
- `amd64`
- `arm64`
- `arm`
- `386`

The `OS` flag can be:
- `darwin`
- `linux`
- `windows`


## Compile the agent with custom default values

1. Generate the configuration file:
```bash
tealc compile --drop-env > ./env.txt
```

This configuration file can then be modified to set custom defaults before recompilation.

```yaml
# Optional seed used during agent compilation/obfuscation.
CLIENT__COMPILE_SEED=

# Shared secret used by the server to authenticate clients.
# Keep this value private and generate a strong random token.
COMMON__ACCESS_TOKEN=

# Public age key corresponding to the server's private key
AGENT__AGEPUBKEY=age1e4txlmjtmc4sx5f8s7fhpka64d4d05rj3qn3jy4tgrta4p22euvq00ac5p

# HTTP domain
HTTP_DOMAIN=www.example.com
# Domain used to perform SSH over TLS
TLS_DOMAIN=app.example.com
# Primary domain used for SSH-over-DNS transport
DNS_DOMAIN=t.example.com

# Secondary SSH-over-DNS domain (experimental).
# Prefer DNS_DOMAIN unless you have a specific reason to use this.
DNS_DOMAIN_ALT=s.example.com
# SSHD port exposed by the server
SSHD_PORT=22222
# HTTP Port
HTTP_PORT=80
# HTTPS port
HTTPS_PORT=443
# DNS Port
DNS_PORT=53
# Quic Port
QUIC_PORT=443
# Whether generated agents should be compressed (true|false).
# Leave empty to use the default behavior.
COMPRESS=
# Either "http" or "https", depending on how the server is exposed.
AGENT__SERVER_SCHEME=http

```

```bash
tealc compile --env [/PATH/TO/ENV]
```