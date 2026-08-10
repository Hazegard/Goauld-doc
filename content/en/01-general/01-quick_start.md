---
title: Quick start
description: Goauld quick start guide
weight: 1
---

This setup involves three components running on separate machines:

- **Server**: publicly reachable machine
- **Client (tealc)**: operator machine
- **Agent**: target machine

Prebuilt client and server binaries are available at [https://github.com/Hazegard/Goauld/releases](https://github.com/Hazegard/Goauld/releases).

See [general/compilation]({{< ref "01-general/02-compilation" >}}) to compile the components.

## Generate secrets

Install [age](https://github.com/filosottile/age), then run:

```bash
$ age-keygen
# created: 2026-03-27T21:44:18+01:00
# public key: age1krjxdnhmf2kqm8rdhyf6sr5nfvlwdcslux3fxt8amcrncwn3ss9sydlvd0
AGE-SECRET-KEY-1NJ4DRPNKNGEVFK50JHUD6RKZ3NJ3Q9S5KYMTARTLXU0P0KQU8AAQNE4C2F
```

The private key (`AGE-SECRET-KEY-*`) is used by the server, and the public key is embedded in agents.


## Generate access token (and admin token)

```bash
openssl rand -base64 42
```

At this point, you should have:

- Age private key → used by server
- Age public key → used by client/agent
- Access token → shared between server and client


## Server

A minimal configuration file that exposes only HTTP and SSH services is:

```yaml
#Age private key used by the server.
age-privkey: "AGE-SECRET-KEY-1NJ4DRPNKNGEVFK50JHUD6RKZ3NJ3Q9S5KYMTARTLXU0P0KQU8AAQNE4C2F"

# Domains used to serve HTTP and WebSocket traffic.
http-domain:
- www.example.com

# Address and port to bind for HTTP connections (port 0 = random).
http-listen-addr: :80

# Address and port to bind for SSH connections (port 0 = random).
ssh-listen-addr: :22222

# Access token required for the /manage/ API endpoint.
access-token:
- XXXXXXXXXX

# Disable TLS: we only listen over plain HTTP as minimal example
tls: false
```

Start the server:

```bash
./goauld_server --config-file config.yaml
```

See [server/deployment]({{< ref "03-server/02-deployment" >}}) to install and deploy the server with all options (TLS, DNS server, etc...).



## Tealc configuration (client)

Place the tealc configuration file in `$HOME/.config/tealc.yaml`.

```yaml
# Access token required to access the /manage/ endpoint.
access-token: XXXXXXXXXX

# HTTP Server to connect to.
server: http://www.example.com

# SSH Server to connect to.
ssh-server: example.com:22222

# To generate agents
age-public-key: age1krjxdnhmf2kqm8rdhyf6sr5nfvlwdcslux3fxt8amcrncwn3ss9sydlvd0
```

## Agent

### Generate an agent

1. Generate the configuration file:
```bash
tealc compile --drop-env > ./env.txt
```

2. Update the configuration file accordingly to your setup

Minimal file with only HTTP (and SSHD) enabled:
```yaml
# Public age key corresponding to the server's private key
AGENT__AGEPUBKEY=age1e4txlmjtmc4sx5f8s7fhpka64d4d05rj3qn3jy4tgrta4p22euvq00ac5p
# HTTP domain
HTTP_DOMAIN=www.example.com
# SSHD port exposed by the server
SSHD_PORT=22222
# HTTP Port
HTTP_PORT=80
```

3. Compile the agent using the generated configuration file:
```bash
tealc compile --env ./env.txt --id agent --goarch amd64 --goos windows
```

The compiled agent will be located in the folder `output/agent/` (e.g.: `output/agent/goauld_windows-amd64.exe`)

See [client/compilation]({{< ref "04-client/12-compilation" >}}) for more compilation options.

### Execute the agent:

```powershell
.\goauld_windows-amd64.exe
```

If execution is successful:
- The agent appears in `tealc tui`
- You can connect using `tealc ssh`

You can also reconfigure on the fly the agent via cli, environment variables or configuration file (see [general/variables]({{< ref "01-general/03-variables" >}})).



## Connect to an agent

1. Via the TUI
```bash
tealc tui
```

Then select the agent you want to connect to.

2. Direct
```bash
tealc ssh [AGENT_NAME]
```

Where AGENT_NAME is by default `[USERNAME]@[HOSTNAME]`