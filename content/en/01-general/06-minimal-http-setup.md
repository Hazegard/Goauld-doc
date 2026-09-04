---
title: Minimal HTTP(S)-only setup
description: Run Goauld with only HTTP(S) exposed
weight: 6
---

This setup is useful when the server can expose only an HTTP/HTTPS service. It
does not require a public SSH, TLS, QUIC, or DNS listener. The agent uses the
HTTP server for its control channel and SSH-over-WebSocket for its data
channel; the operator uses the management API and the SSH-over-WebSocket
endpoint through the same HTTP(S) service.

## Server

Enable HTTP and HTTPS, enable the operator SSH-over-WebSocket bridge, and
disable the other listeners:

```yaml
age-private-key: AGE-SECRET-KEY-...

http-domain:
  - goauld.example.com
http-listen-addr: :80
https-listen-addr: :443

tls: true
ssh: false
quic: false
dns: false
ssh-websocket: true

access-token:
  - ACCESS_TOKEN
admin-token:
  - ADMIN_TOKEN
allowed-ips:
  - 203.0.113.10/32 # replace with each operator's public IP/CIDR
```

Configure the TLS certificate with `tls-key`/`tls-cert`, or configure
`letsencrypt-email` and use the server's certificate automation. The server
must be reachable at the configured `http-domain` from both the agent and the
operator.

`http-listen-addr` serves plain HTTP; `https-listen-addr` serves HTTPS,
including `wss://` and the management API. `--ssh-websocket` protects
`/ssh-ws/` with the access token and IP allowlist.

`allowed-ips` is required for operator access: an empty list denies the
management API and SSH connections. Replace the example address with the
public IP address or CIDR of every operator workstation.

## Agent

Configure the agent's control server and select WebSocket as its SSH transport
order. `--ws` is a client option; on the agent the equivalent is `-O WS` or
`--rssh-order WS`:

```yaml
server: https://goauld.example.com
rssh-order:
  - WS
sshd-enabled: true
```

The agent's `server` value is used for the control channel and the
agent-facing `/wssh/{agent-id}` data endpoint. `sshd-enabled` enables the
agent SSH service, which is reached through the existing outbound WebSocket
tunnel; it does not open an inbound listener on the target.

If DNS is also required for restricted environments, enable the server's DNS
listener and add `DNS` after `WS`:

```yaml
rssh-order: [WS, DNS]
dns-server:
  - dns.example.com:53
dns-domain: t.example.com
```

DNS is a slower fallback than WebSocket.

## Client

Use the HTTPS server address and enable the client-side WebSocket option:

```yaml
server: https://goauld.example.com
ws: true
access-token: ACCESS_TOKEN
admin-token: ADMIN_TOKEN
```

Then connect normally:

```sh
tealc ssh AGENT_NAME
```

The client-side `--ws` flag selects `/ssh-ws/` for its SSH connection. The
agent-side `-O WS` selects `/wssh/{agent-id}` for the agent's outbound SSH
connection; these are different WebSocket paths with different roles.

See [Listening services]({{< ref "03-server/01-services" >}}),
[Access control]({{< ref "03-server/03-access_control" >}}), and
[Tunneling]({{< ref "02-agent/01-tunnels" >}}) for the complete setup.
