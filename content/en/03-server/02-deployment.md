---
title: Deployment
description: Goauld server deployment
weight: 2
---

> [!WARNING]
> Goauld is currently not designed to run behind a reverse proxy.
> In particular, the whitelisting feature will not work behind a reverse proxy.

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
      - "X.X.X.X:53:53/tcp"
      - "X.X.X.X:53:53/udp"
      - "X.X.X.X:80:80"
      - "X.X.X.X:443:443"
      - "X.X.X.X:2222:2222"
    volumes:
      - ./certmagic:/root/.local/share/certmagic
      - ./Goauld.db:/app/Goauld.db
      - ./server_config.yaml:/app/server_config.yaml
      - ./binaries:/app/binaries
```

> [!WARNING]
> This example maps host port `22222`, which only works if `ssh-listen-addr` is explicitly set to `:22222` in the configuration file. The compiled-in default (and the sample configuration file shipped with the project) use `:2222` instead. Make sure the port mapping above and your `ssh-listen-addr` value actually agree, in either direction.

## configuration file example

```yaml
#Age private key used by the server.
age-privkey: ""

# Domains used to serve HTTP and WebSocket traffic.
http-domain:
- www.example.com

# Domains used to serve raw TLS traffic (SSH over TLS).
tls-domain:
- app.example.com

# Domain used to serve DNS-based traffic (SSH over DNS).
dns-domain: t.example.com

# Domain used to serve DNS-based traffic (SSH over DNS-ALT).
dns-domain-alt: s.example.com

# Address and port to bind for HTTP connections (port 0 = random).
http-listen-addr: :80

# Address and port to bind for HTTPS connections (port 0 = random).
https-listen-addr: :443

# Address and port to bind for SSH connections (port 0 = random).
ssh-listen-addr: :2222

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
letsencrypt-mail: mail@example.com

# Enable QUIC protocol support.
quic: true

# Enable DNS server for SSH-over-DNS connections.
dns: true

# Disable database usage.
# Note: despite the field name/comment, this is the *enabled* (on-disk) state.
# See server/database for the actual flag that disables on-disk persistence.
db: false

# Path or filename of the database to use.
db-file-name: Goauld.db

# List of IP addresses allowed to access the /manage/ endpoint.
# Note: only exact IP addresses are matched; CIDR ranges (e.g. "0.0.0.0/32") are
# accepted by the parser but are NOT expanded/enforced as ranges at request time
# (see server/access_control#ip-allowlisting).
allowed-ips:
- 127.0.0.1
- 192.168.1.1

# Access token required for the /manage/ API endpoint.
access-token:
- TODO_TOKEN

# Admin token required for the /admin/ API endpoint.
admin-token:
- TODO_TOKEN

# HTTP Basic Auth credentials required to access the binaries endpoint.
binaries-basic-auth: username:password

# Filesystem path where agent binaries are stored.
binaries-path-location: ./binaries
```

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

If the goauld server receive as source IP the docker gateway IP:

```bash
iptables -t nat -I PREROUTING -p tcp --dport "$SSHD_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p tcp --dport "$HTTP_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p tcp --dport "$HTTPS_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p udp --dport "$DNS_PORT" -m addrtype --dst-type LOCAL -j DOCKER
iptables -t nat -I PREROUTING -p tcp --dport "$DNS_PORT" -m addrtype --dst-type LOCAL -j DOCKER
```