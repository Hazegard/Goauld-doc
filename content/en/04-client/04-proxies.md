---
title: Proxies
description: Goauld client local proxies
weight: 4
---


The client exposes the agent's proxies on the loopback interface (localhost).

## SOCKS proxy

Exposed by default on the port `1080`.

See [agent/proxies#socks-proxy]({{< ref "02-agent/02-proxies" >}}#socks-proxy) for details about the SOCKS proxy.


## HTTP proxy

Exposed by default on the port `3128`.

See [agent/proxies#http-proxy]({{< ref "02-agent/02-proxies" >}}#http-proxy) for details about the HTTP proxy.


## HTTP proxy with NTLM/Kerberos application-level authentication

Exposed by default on the port `3129`.

See [agent/proxies#http-proxy-with-ntlmkerberos-authentication]({{< ref "02-agent/02-proxies" >}}#http-proxy-with-ntlmkerberos-authentication) for details about the MITM HTTP proxy.


## WireGuard

Exposed by default on the port `51820`.

See [client/wireguard]({{< ref "04-client/11-wireguard" >}}) for configuration options and usage details.


## Configurable ports and toggles

All proxies are configurable via flags on the `tealc ssh` command:
- `--[no-]socks`, `--socks-port=1080`: enable/disable SOCKS and set its port
- `--[no-]http`, `--http-port=3128`: enable/disable HTTP and set its port
- `--[no-]http-mitm`, `--http-mitm-port=3129`: enable/disable MITM HTTP and set its port
- `--[no-]wg`, `--wg-port=51820`: enable/disable WireGuard and set its port

See [client/ssh]({{< ref "04-client/03-ssh" >}}) for the full flag reference.