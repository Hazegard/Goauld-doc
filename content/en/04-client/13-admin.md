---
title: Admin features
description: Goauld client admin features
weight: 13
---

> [!NOTE]
> `tealc admin` is for internal/administrative use only, rarely needed in normal operation.

## Dump the current configuration

```bash
tealc admin config
```

```yaml
# Age private key used by the server.
age-private-key: "[REDACTED]"

# Domains used to serve HTTP and WebSocket traffic.
http-domain:
- 0.0.0.0

# Domains used to serve raw TLS traffic (SSH over TLS).
tls-domain:
- localhost

# Domain used to serve DNS-based traffic (SSH over DNS).
dns-domain: t.example.com

# Address and port to bind for HTTP connections (port 0 = random).
http-listen-addr: 0.0.0.0:80

# Address and port to bind for HTTPS connections (port 0 = random).
https-listen-addr: 0.0.0.0:443

# Address and port to bind for SSH connections (port 0 = random).
sshd-listen-addr: 0.0.0.0:2222

# Address and port to bind for DNS connections (port 0 = random).
dns-listen-addr: 0.0.0.0:53

# Address and port to bind for QUIC connections (port 0 = random).
quic-listen-addr: 0.0.0.0:443

# Enable TLS support.
tls: true

# Path to the TLS private key file.
tls-key: ./local_config/cert/local.key

# Path to the TLS certificate file.
tls-cert: ./local_config/cert/local.pem

# Email used when generating Let's Encrypt certificates.
letsencrypt-email: mail@example.com

# Enable QUIC protocol support. Requires tls to also be enabled.
quic: true

# Enable DNS server for SSH-over-DNS connections.
dns: true

# Disable database usage.
no-db: false

# Path or filename of the database to use.
db-file-name: Goauld.db

# List of IP addresses allowed to access the /admin/ and /manage/ endpoints, SSH password authentication, and SSH local port forwarding.
allowed-ips: []

# List of reverse-proxy/load-balancer IP addresses or CIDR ranges trusted to set X-Forwarded-For. Only set this when Goauld is deployed behind a reverse proxy. Must include the proxy's own IP/CIDR. Leave empty (default) for direct internet-facing deployments; X-Forwarded-For is otherwise ignored. Only affects the allowed-ips check for the /admin/, /manage/, and /ssh-ws/ HTTP endpoints. Direct raw SSH password authentication and SSH local port forwarding remain restricted by allowed-ips, but use the TCP peer address; trusted-proxies does not apply to them.
trusted-proxies: []

# Access token required for the /manage/ API endpoint.
access-token:
- "[REDACTED]"

# Admin token required for the /admin/ API endpoint.
admin-token:
- "[REDACTED]"

# HTTP Basic Auth credentials required to access the binaries endpoint.
binaries-basic-auth: "[REDACTED]"

# Filesystem path where agent binaries are stored.
binaries-path: ./binaries

# Increase log verbosity. Repeat for more detail.
verbose: 3

# Suppress all log output.
quiet: false

# Show version information and exit.
version: false

# Generate a configuration file from the current settings.
generate-config: false

# Path to the configuration file to use.
config-file: ./local_config/server.yaml

```

## Dump the connected agent information

```bash
tealc admin dump
```

```yaml
- id: 1ec1bd83de498e7da8852efe6d8c16c5
  name: user@Archamd2
  SSHMode: WS
  usedPorts: "0"
  lastUpdated: 2026-03-09T17:31:43.426417+01:00
  lastPing: 2026-03-09T17:32:22.788485+01:00
  platform: darwin
  architecture: arm64
  username: user
  hostname: Archamd2
  IPs: 172.16.22.101/24,192.168.29.1/24,172.16.40.1/24,192.168.139.3/23,fd07:b51a:cc66:0:a617:db5e:ab7:e9f1/64,10.10.20.19/24
  path: /Users/user/Library/Caches/go-build/c0/c0ece0fea100ea19cf4edec893a6caafb9f6b1f91c6f47ef3a1bab62d0455bbe-d/agent
  remoteAddr: 127.0.0.1
  TLSSH:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
  QUIC:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
  WSSH:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
    sshConn:
      localAddr: 127.0.0.1:56748
      remoteAddr: 127.0.0.1:2222
    wsConn:
      localAddr: 127.0.0.1:80
      remoteAddr: 127.0.0.1:56747
  SSHTTP:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
  socketIO:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
    socketId: ltHwpaxDrVu0beeBAAAB
    connected: true
  ssh:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
    SSHConnection:
    - agentId: 1ec1bd83de498e7da8852efe6d8c16c5
      sshConn:
        localAddr: 127.0.0.1:2222
        remoteAddr: 127.0.0.1:56748
      clientVersion: SSH-2.0-Goauld-dev
      sessionID: d7421e2ee9f35a1a6bf4e6908b0e7d19426565f85efc76d8458f5d2d39b55f33
      serverVersion: SSH-2.0-OpenSSH_8.7
    - agentId: 1ec1bd83de498e7da8852efe6d8c16c5
      sshConn:
        localAddr: 127.0.0.1:2222
        remoteAddr: 127.0.0.1:56748
      clientVersion: SSH-2.0-Goauld-dev
      sessionID: d7421e2ee9f35a1a6bf4e6908b0e7d19426565f85efc76d8458f5d2d39b55f33
      serverVersion: SSH-2.0-OpenSSH_8.7
    - agentId: 1ec1bd83de498e7da8852efe6d8c16c5
      sshConn:
        localAddr: 127.0.0.1:2222
        remoteAddr: 127.0.0.1:56748
      clientVersion: SSH-2.0-Goauld-dev
      sessionID: d7421e2ee9f35a1a6bf4e6908b0e7d19426565f85efc76d8458f5d2d39b55f33
      serverVersion: SSH-2.0-OpenSSH_8.7
    SSHListeners:
    - 127.0.0.1:56749
    - 127.0.0.1:56751
    - 127.0.0.1:56752
  dns:
    agentId: 1ec1bd83de498e7da8852efe6d8c16c5
```

## Update the server log level

```bash
tealc admin log-level debug
```

The level is a required argument (e.g. `debug`, `info`, `warn`, `error`).

## Dump the full server state

```bash
tealc admin state
```

Displays the full server state, including connected agents and the current configuration.

## JSON output

`dump`, `config` and `state` all support `--json` to print JSON instead of colorized YAML.
