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

The server uses the private key (prefixed with `AGE-SECRET-KEY-`). Agents use the public key (prefixed with `age1`).


## Generate access token (and admin token)

```bash
openssl rand -base64 42
```

Run this command again (or reuse the same method) to generate a separate admin token, used to protect the `/admin/` API endpoint.

At this point, you should have:

- age private key → used by server
- age public key → used by client/agent
- Access token → shared between server and client
- Admin token → shared between server and client

> [!NOTE]
> The server, agent, and client all support `--generate-config`, which writes out a configuration file from the currently-resolved settings (CLI flags, environment variables, etc.). This can be used instead of hand-writing the YAML files shown below.

## Server

A traditional minimal configuration file that exposes HTTP and a direct SSH
listener is:

```yaml
# The server's age private key.
age-private-key: "AGE-SECRET-KEY-1NJ4DRPNKNGEVFK50JHUD6RKZ3NJ3Q9S5KYMTARTLXU0P0KQU8AAQNE4C2F"

# Domains used to serve HTTP and WebSocket traffic.
http-domain:
- www.example.com

# Address and port to bind for HTTP connections (port 0 = random).
http-listen-addr: :80

# Address and port to bind for SSH connections (port 0 = random).
sshd-listen-addr: :2222

# Access token required for the /manage/ API endpoint.
access-token:
- XXXXXXXXXX

# Admin token required for the /admin/ API endpoint.
admin-token:
- XXXXXXXXXX

# Operator IP addresses allowed to use the management API and SSH.
# Replace with the public IP/CIDR of each operator workstation.
allowed-ips:
- 203.0.113.10/32

# Disable TLS (minimal example, do not use in production)
tls: false

# Disable the DNS listener (not used in this minimal example)
dns: false
```

Start the server:

```bash
./goauld_server --config-file config.yaml
```

See [server/deployment]({{< ref "03-server/02-deployment" >}}) to install and deploy the server with all options (TLS, DNS server, etc...).

For deployments where only HTTP/HTTPS can be exposed, use the
[minimal HTTP(S)-only setup]({{< ref "01-general/06-minimal-http-setup" >}}).



## Tealc configuration (client)

Place the tealc configuration file in `$HOME/.config/tealc.yaml`.

```yaml
# Access token required to access the /manage/ endpoint.
access-token: XXXXXXXXXX

# HTTP Server to connect to.
server: http://www.example.com

# SSH Server to connect to.
ssh-server: example.com:2222

# Agent age public key. Only consumed by `tealc compile` when generating agents.
age-public-key: age1krjxdnhmf2kqm8rdhyf6sr5nfvlwdcslux3fxt8amcrncwn3ss9sydlvd0
```

> [!NOTE]
> `age-public-key` is used only by `tealc compile`. Instead of `tealc.yaml`, you can pass
> it per build with `-K`/`--age-public-key` (or `TEALC_AGE_PUBLIC_KEY`), or bake it into a
> build file as `AGENT__AGE_PUBLIC_KEY` (which is what the next section does).

## Agent

### Generate an agent

1. Generate the configuration file:
```bash
tealc compile --drop-env > ./env.txt
```

2. Update the configuration file according to your setup

Minimal file with only HTTP (and SSHD) enabled:
```bash
# Public age key corresponding to the server's private key
AGENT__AGE_PUBLIC_KEY=age1krjxdnhmf2kqm8rdhyf6sr5nfvlwdcslux3fxt8amcrncwn3ss9sydlvd0
# HTTP domain
HTTP_DOMAIN=www.example.com
# SSHD port exposed by the server
SSHD_PORT=2222
# HTTP Port
HTTP_PORT=80
```

3. Compile the agent using the generated configuration file:
```bash
tealc compile --env ./env.txt --id agent --goarch amd64 --goos windows
```

The compiled agent will be located in the folder `output/agent/` (e.g.: `output/agent/goauld_windows-amd64.exe`)

See [client/compilation]({{< ref "04-client/12-compilation" >}}) for more compilation options.

### Execute the agent

```powershell
.\goauld_windows-amd64.exe
```

If execution is successful:
- The agent appears in `tealc tui`
- You can connect using `tealc ssh`

> [!NOTE]
> `--background`/`-B` runs the agent as a detached background process instead of in the foreground.

You can also reconfigure the agent on the fly via cli, environment variables or configuration file (see [general/variables]({{< ref "01-general/03-variables" >}})).



## Connect to an agent

- Via the TUI
```bash
tealc tui
```

Then select the agent you want to connect to.

- Direct
```bash
tealc ssh [AGENT_NAME]
```

Where AGENT_NAME is by default `[USERNAME]@[HOSTNAME]`, unless overridden with `--name` at run-time, or with `AGENT__NAME=...` in the env file passed to `tealc compile --env` at compile-time.
