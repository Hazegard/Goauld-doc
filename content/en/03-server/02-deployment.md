---
title: Deployment
description: Goauld server deployment
weight: 2
---

> [!NOTE]
> IP allowlisting (`--allowed-ips`) can work correctly behind a reverse proxy via `--trusted-proxies` (see [Reverse proxy support]({{< ref "03-server/03-access_control" >}}#reverse-proxy-support) in Access control). This only covers the `/admin/` and `/manage/` HTTP endpoints; SSH password authentication and SSH local port forwarding always see the raw TCP peer address, so a proxied SSH listener is still not supported.

## Docker compose example

```yaml
services:
  goauld_server:
    build:
      context: .
      args:
        - COMPRESS=1
    # platform: linux/amd64
    container_name: server
    image: goauld_server
    ports:
      - "X.X.X.X:53:53/udp"
      - "X.X.X.X:80:80"
      - "X.X.X.X:443:443"
      - "X.X.X.X:443:443/udp" # QUIC transport
      - "X.X.X.X:2222:2222"
    volumes:
      - ./certmagic:/root/.local/share/certmagic # certmagic: Let's Encrypt certificate cache, persist across restarts
      - ./Goauld.db:/app/Goauld.db
      - ./server_config.yaml:/app/server_config.yaml
      - ./binaries:/app/binaries
```

> [!NOTE]
> The SSH port mapping above must match your `sshd-listen-addr` value (`:2222` by default).

## Server identity

The server needs an Age private key; it is the only strictly required setting.

- `--age-private-key`: the Age private key used by the server. The agents are compiled with the matching public key (`--age-public-key`), and a mismatch means agents cannot register.

> [!WARNING]
> Treat this key as a secret. Anything holding it can impersonate the server to every agent built against its public key.

## Configuration file example

```yaml
# Age private key used by the server.
age-private-key: ""

# Domains used to serve HTTP and WebSocket traffic.
http-domain:
- www.example.com

# Domains used to serve raw TLS traffic (SSH over TLS).
tls-domain:
- app.example.com

# Domain used to serve DNS-based traffic (SSH over DNS).
dns-domain: t.example.com

# Address and port to bind for HTTP connections (port 0 = random).
http-listen-addr: :80

# Address and port to bind for HTTPS connections (port 0 = random).
https-listen-addr: :443

# Address and port to bind for SSH connections (port 0 = random).
sshd-listen-addr: :2222

# Address and port to bind for DNS connections (port 0 = random).
dns-listen-addr: :53

# Address and port to bind for QUIC connections (port 0 = random).
quic-listen-addr: :443

# Enable TLS support.
tls: true

# Path to the TLS private key file.
tls-key: ""

# Path to the TLS certificate file.
tls-cert: ""

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
# Individual IPs and CIDR ranges are both supported, and this list is fail-closed:
# an empty list denies everyone, on all access paths (see 03-server/03-access_control#ip-allowlisting).
allowed-ips:
- 127.0.0.1
- 192.168.1.0/24

# List of reverse-proxy/load-balancer IP addresses or CIDR ranges trusted to set X-Forwarded-For.
# Only set this when Goauld is deployed behind a reverse proxy: must include the proxy's own IP/CIDR.
# Leave empty (default) for direct internet-facing deployments; X-Forwarded-For is otherwise ignored.
# Only affects the allowed-ips check above for the /admin/ and /manage/ HTTP endpoints, SSH password
# authentication and SSH local port forwarding are not covered (see 03-server/03-access_control#reverse-proxy-support).
trusted-proxies: []

# Access token required for the /manage/ API endpoint.
access-token:
- EXAMPLE_TOKEN

# Admin token required for the /admin/ API endpoint.
admin-token:
- EXAMPLE_TOKEN

# HTTP Basic Auth credentials required to access the binaries endpoint.
binaries-basic-auth: username:password

# Filesystem path where agent binaries are stored.
binaries-path: ./binaries
```

## Reloading TLS certificates and domains

Sending the server process a `SIGHUP` signal reloads TLS-related configuration without restarting, keeping active tunnels alive:

```bash
kill -HUP [SERVER_PID]
```

> [!NOTE]
> SIGHUP reload only applies when TLS is enabled (`--tls`). If the server runs with `--no-tls`, SIGHUP has no effect.

**With a custom certificate** (`--tls-cert`/`--tls-key`): The certificate and key files are reloaded from disk.

**With Let's Encrypt** (default): The `--http-domain` and `--tls-domain` configuration is re-read, updating the set of domains the certificate covers.

## DNS configuration

Three DNS records are required:

- example.com is your domain
- t.example.com is the subdomain handling the tunnel over DNS (should be as short as possible)
- tns.example.com is the NS subdomain

```txt
NS record: t => tns.example.com
A  record: t => "$IP"
A  record: tns => "$IP"
```

## Docker iptables

If the Goauld server receives the Docker gateway IP as its source IP:

```bash
iptables -t nat -I PREROUTING -p tcp --dport "$SSHD_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p tcp --dport "$HTTP_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p tcp --dport "$HTTPS_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p udp --dport "$DNS_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p udp --dport "$QUIC_PORT" -m addrtype --dst-type LOCAL -j DOCKER
```