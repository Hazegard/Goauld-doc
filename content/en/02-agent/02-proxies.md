---
title: Exposed proxies
description: Goauld agent proxies
weight: 2
---

The agent exposes three proxies that allow interaction with the host's network:
- An HTTP proxy
- An HTTP proxy that performs NTLM/Kerberos application-level authentication
- A SOCKS proxy

> [!NOTE]
> NTLM/Kerberos authentication requires intercepting traffic (MITM) to inject authentication headers. This is implemented in a dedicated proxy for isolation.


## HTTP proxy

For each incoming request, the HTTP proxy determines whether an upstream proxy should be used and which one.

By default, it uses the same proxy selection priority as the agent (see [agent/tunnels#egress-proxies]({{< ref "02-agent/01-tunnels" >}}#egress-proxies)). This behavior can be customized using:

### Flags

- `--http-custom-proxy`: override the system proxy
- `--http-proxy-username`: Username to authenticate on the proxy
- `--http-proxy-password`: Password to authenticate on the proxy
- `--http-proxy-domain`: Domain to authenticate on the proxy
- `--http-proxy-enabled`/`--no-http-proxy-enabled`: Enable/Disable the HTTP proxy
- `--http-port`: the remote port the HTTP proxy binds to on the server side (default: `0`, meaning a random port is chosen).

> [!NOTE]
> This proxy is automatically enabled in these cases:
> - MITM HTTP proxy is enabled
> - SOCKS proxy is configured to use the HTTP proxy

## HTTP proxy with NTLM/Kerberos authentication

Some applications require NTLM/Kerberos authentication.

This proxy transparently handles NTLM/Kerberos authentication requests, exposing network access to the client as if requests originated from the host.

It uses the previously described HTTP proxy as its upstream proxy to handle system proxy and proxy authentication requirements.

> [!WARNING]
> This proxy is only available on Windows.



### Flags

- `--mitm-http-proxy-enabled`/`--no-mitm-http-proxy-enabled`: Enable/Disable the MITM HTTP proxy
- `--mitm-http-port`: the remote port the MITM HTTP proxy binds to on the server side (default: `0`, meaning a random port is chosen).
- `--mitm-http-proxy-username`: Username for the MITM HTTP upstream proxy
- `--mitm-http-proxy-password`: Password for the MITM HTTP upstream proxy
- `--mitm-http-proxy-domain`: Domain for the MITM HTTP upstream proxy

> [!NOTE]
> This proxy is automatically enabled when SOCKS proxy is configured to use MITM mode.

## SOCKS proxy

The SOCKS proxy can be configured to use different HTTP upstream proxies:

- `http`: agent HTTP upstream proxy
- `mitm`: agent HTTP with NTLM/Kerberos upstream proxy
- `system`: the default HTTP proxy configured on the host (the proxy used by the agent to access the Goauld server)
- `custom`: the custom proxy configured in the `--socks-custom-proxy` flag
- `none`: no upstream proxy

### Flags

- `--socks-custom-proxy`: Custom upstream HTTP proxy used within the SOCKS proxy
- `--socks-upstream-proxy`: Configure the upstream HTTP proxy to use (none|system|http|mitm|custom)
- `--socks-proxy-username`: Username for the SOCKS upstream proxy
- `--socks-proxy-password`: Password for the SOCKS upstream proxy
- `--socks-proxy-domain`: Domain for the SOCKS upstream proxy
- `--socks-enabled`/`--no-socks-enabled`: Enable/Disable the SOCKS proxy
- `--socks-port`: the remote port the SOCKS proxy binds to on the server side (default: `0`, meaning a random port is chosen).

## Remote port forwarding

Independent of the three proxies above, the agent can forward specific ports on its host to the server, one at a time.

### Flags

- `-R`/`--remote-port-forwarding`: `REMOTE_PORT[:LOCAL_IP]:LOCAL_PORT`: forwards `LOCAL_IP:LOCAL_PORT` (on the agent's host) to `REMOTE_PORT` on the server. `LOCAL_IP` defaults to `127.0.0.1` if omitted. Use `0` for `REMOTE_PORT` to let the server choose a random port. Can be repeated (or comma-separated) to forward multiple ports.

Example: `--remote-port-forwarding 8080::3000` forwards `127.0.0.1:3000` on the agent to port `8080` on the server.
