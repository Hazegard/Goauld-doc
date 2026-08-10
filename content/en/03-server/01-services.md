---
title: Listening services
description: Goauld listening services
weight: 1
---

To allow agents to tunnel SSH connections over different transports, the server must expose the corresponding service, then decapsulate the traffic and forward it to the SSHD server.


> [!NOTE]
> For all listeners, the listen address flag has the following format:
> `[IP]:[PORT]`
>
> If no IP address is provided, the service will listen on all interfaces.
> However, the `:` is still required before the port

## SSH

No encapsulation, directly exposed.

### Flags


- `--ssh-listen-addr`: The address the SSHD server will listen to. Format: `[IP]:[PORT]`.


## TLS


> [!NOTE]
> TLS configuration impacts both the SSH over TLS tunnel and the HTTPS web server.


### Flags

- `--https-listen-addr`: the address the TLS server will listen to. Format: `[IP]:[PORT]`.
- `--http-domain`: the domain(s) on which the web server will respond (accepts a comma-separated list of multiple domains).
- `--tls-domain`: the domain(s) on which the SSH over TLS listener will respond (accepts a comma-separated list of multiple domains).
- `--tls`/`--no-tls`: Enable/Disable the TLS listener. Note that it impacts both the HTTPS web server and the SSH over TLS listener.

### TLS keys

The server allows two ways of providing the TLS certificate:
- Either provide a custom certificate
  - `--tls-key`: path to the TLS certificate key
  - `--tls-cert`: path to the TLS certificate
- Let the server handle the certificate:
  - `--letsencrypt-mail`: the mail used by the ACME protocol

> [!NOTE]
> With a custom certificate, the same certificate must be able to handle both domains. This doesn't apply to the default Let's Encrypt setup, which issues a separate certificate per domain on demand.

## QUIC

### Flags

- `--quic-listen-addr`: the address the QUIC server will listen to. Format: `[IP]:[PORT]`.
- `--quic`/`--no-quic`: Enable/Disable the QUIC listener. Requires `--tls` to also be enabled: the QUIC listener does not start otherwise.

> [!NOTE]
> If required to open traffic (firewalls), this listener always listens on UDP

## HTTP

### Flags

- `--http-listen-addr`: the address the plain HTTP server (serving both the HTTP and Websocket transports) will listen to. Format: `[IP]:[PORT]`.

SSH over HTTP is available at: `http://[HTTP_DOMAIN]/sshttp/` and `https://[HTTP_DOMAIN]/sshttp/`

> [!NOTE]
> The secure `https://` variant depends on the TLS listener, not `--http-listen-addr`: it requires `--https-listen-addr`, `--tls`, and `--http-domain` to be configured (see [TLS](#tls) above).

## Websocket

The Websocket listener is served by the same HTTP server as the [HTTP](#http) listener above, and shares its `--http-listen-addr` configuration. There is no separate flag dedicated to the Websocket listener.

SSH over WebSocket is available at: `ws://[HTTP_DOMAIN]/wssh/` and `wss://[HTTP_DOMAIN]/wssh/`

> [!NOTE]
> As with HTTP, the secure `wss://` variant depends on the TLS listener (`--https-listen-addr`, `--tls`, `--http-domain`), not `--http-listen-addr`.


## DNS

The DNS server acts as an authoritative server and responds to DNS queries that match the SSH-over-DNS format.

### Flags

- `--dns-listen-addr`: DNS server listen address. Use port 53 for compatibility with recursive DNS resolvers.
- `--dns-domain`: DNS domain for queries. Use the shortest domain possible to maximize throughput.
- `--dns`/`--no-dns`: Enable/Disable the DNS listener.

## Internal control channel

The HTTP server exposes an internal `/live/{agentId}/` Socket.IO control channel. Every connected agent uses this channel, regardless of its tunneling transport method.

The control channel shares the same `--http-listen-addr` and `--https-listen-addr` listeners as the HTTP and WebSocket transports. It is used internally by the server for agent management (kill commands, connection-state tracking) and is not intended for direct operator access.

> [!WARNING]
> There is no separate flag to keep this channel running independently of the HTTP/HTTPS listeners. Since every connected agent depends on it regardless of tunneling transport, an `--http-listen-addr` that isn't reachable (combined with `--no-tls`) prevents all agents from registering or being managed.

## Flag summary

| Transport | Flag                     | Description                    | Example         |
| --------- | ------------------------ | ------------------------------- | --------------- |
| SSH       | `--ssh-listen-addr`      | Address for SSH listener        | `[IP]:[PORT]`   |
| TLS       | `--https-listen-addr`    | TLS server listen address       | `[IP]:[PORT]`   |
| TLS       | `--tls` / `--no-tls`     | Enable/disable TLS listener     | `--tls`         |
| TLS       | `--http-domain`          | HTTPS web server domain         | `example.com`   |
| HTTP / Websocket | `--http-listen-addr` | HTTP server listen address (serves both transports) | `[IP]:[PORT]` |
| QUIC      | `--quic-listen-addr`     | QUIC listener address (UDP)     | `[IP]:[PORT]`   |
| QUIC      | `--quic` / `--no-quic`   | Enable/disable QUIC listener (requires `--tls`) | `--quic` |
| DNS       | `--dns-listen-addr`      | DNS server listen address       | `[IP]:53`       |
| DNS       | `--dns-domain`           | Domain for SSH-over-DNS         | `s.example.com` |
| DNS       | `--dns` / `--no-dns`     | Enable/disable DNS listener     | `--dns`         |
