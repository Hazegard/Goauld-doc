---
title: Listening services
description: Goauld listening services
weight: 1
---

To allow agents to tunnel SSH connection over different transports, the server must expose the corresponding service, then decapsulate the traffic and forward it to the SSHD server.


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
> The TLS configuration both impacts the SSH over TLS tunnel, and the HTTPS web server.


### Flags

- `--https-listen-addr`: the address the TLS server will listen to. Format: `[IP]:[PORT]`.
- `--http-domain`: the domain on which the WebServer will respond.
- `--tls-domain`: the domain on which the SSH over TLS listener will respond.
- `--tls`/`--no-tls`: Enable/Disable the TLS listener. Note that it impacts both the HTTPS webserver and the SSH over TLS listener.

### TLS keys

The server allows two ways of providing the TLS certificate:
- Either provide a custom certificate
  - `--tls-key`: path to the TLS certificate key
  - `--tls-cert`: path to the TLS certificate
- Let the server handle the certificate:
  - `--letsencrypt-mail`: the mail used by the ACME protocol

> [!NOTE]
> The same TLS certificate must be able to handle both domains.

## QUIC

### Flags

- `--quic-listen-addr`
- `--quic`/`--no-quic`: Enable/Disable the QUIC listener

> [!NOTE]
> If required to open traffic (firewalls), this listener always listens on UDP

## Websocket

The Websocket listener is served by the same HTTP server as the [HTTP](#http) listener, and shares its `--http-listen-addr` configuration (see `### Flags` below) — there is no separate flag dedicated to the Websocket listener.

The SSH over Websocket listener listens over the `ws://[HTTP_DOMAIN]/wssh/` and `wss://[HTTP_DOMAIN]/wssh/`

## HTTP

### Flags

- `--http-listen-addr`: the address the plain HTTP server (serving both the Websocket and HTTP transports) will listen to. Format: `[IP]:[PORT]`.

The SSH over HTTP listener listens over the `http://[HTTP_DOMAIN]/sshttp/` and `https://[HTTP_DOMAIN]/sshttp/`


## DNS

The DNS server acts as an authoritative server and responds to DNS queries that match the SSH-over-DNS format.

### Flags

- `--dns-listen-addr`: The address the DNS server will listen to. It is recommended to use the port 53 to be reachable from recursive DNS servers
- `--dns-domain`: The DNS domain on which the DNS server will respond. In order to maximize the throughput, it is recommended to use the shortest domain possible.
- `--dns`/`--no-dns`: Enable/Disable the DNS listener.


| Transport | Flag                     | Description                    | Example         |
| --------- | ------------------------ | ------------------------------- | --------------- |
| SSH       | `--ssh-listen-addr`      | Address for SSH listener        | `[IP]:[PORT]`   |
| TLS       | `--https-listen-addr`    | TLS server listen address       | `[IP]:[PORT]`   |
| TLS       | `--tls` / `--no-tls`     | Enable/disable TLS listener     | `--tls`         |
| TLS       | `--http-domain`          | HTTPS web server domain         | `example.com`   |
| Websocket | `--http-listen-addr`     | HTTP server listen address (also serves Websocket) | `[IP]:[PORT]` |
| HTTP      | `--http-listen-addr`     | HTTP server listen address (also serves Websocket) | `[IP]:[PORT]` |
| QUIC      | `--quic-listen-addr`     | QUIC listener address (UDP)     | `[IP]:[PORT]`   |
| QUIC      | `--quic` / `--no-quic`   | Enable/disable QUIC listener    | `--quic`        |
| DNS       | `--dns-listen-addr`      | DNS server listen address       | `[IP]:53`       |
| DNS       | `--dns-domain`           | Domain for SSH-over-DNS         | `s.example.com` |
| DNS       | `--dns` / `--no-dns`     | Enable/disable DNS listener     | `--dns`         |
