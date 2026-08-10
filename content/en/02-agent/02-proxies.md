---
title: Exposed proxies
description:  Goauld agent proxies
weight: 2
---

The agent exposes three proxies that allow interaction with the host’s network:
- An HTTP proxy 
- An HTTP proxy that performs NTLM/Kerberos application-level authentication
- A SOCKS proxy

> [!NOTE]
> Given that performing NTLM/Kerberos application-level authentication requires to intercept the traffic (MITM) to inject appropriate headers, this feature has been implemented in a dedicated proxy.


## HTTP proxy

For each incoming request, the HTTP proxy determines whether an upstream proxy should be used and which one.


The upstream proxy will be selected by the following priority:

#### Windows

1. `--proxy [PROXY_URL]` (or any other means allowing configuration of the `proxy` flag, see  [general/compilation]({{< ref "01-general/02-compilation" >}}))
2. Environment Variable: `HTTPS_PROXY`, `HTTP_PROXY`, `FTP_PROXY`, or `ALL_PROXY`. `NO_PROXY` is respected.
3. Internet Options: Automatically detect settings (`WPAD`)
4. Internet Options: Use automatic configuration script (`PAC`)
5. Internet Options: Manual proxy server
6. WINHTTP: (`netsh winhttp`)

#### Linux

1. `--proxy [PROXY_URL]` (or any other means allowing configuration of the `proxy` flag, see  [general/compilation]({{< ref "01-general/02-compilation" >}}))
2.  Environment Variable: `HTTPS_PROXY`, `HTTP_PROXY`, `FTP_PROXY`, or `ALL_PROXY`. `NO_PROXY` is respected.

#### macOS

1. `--proxy [PROXY_URL]` (or any other means allowing configuration of the `proxy` flag, see  [general/compilation]({{< ref "01-general/02-compilation" >}}))
2. Environment Variable: `HTTPS_PROXY`, `HTTP_PROXY`, `FTP_PROXY`, or `ALL_PROXY`. `NO_PROXY` is respected.
3. Network Settings: `scutil`

### Flags

By default, the HTTP proxy will use the proxy configuration used by the agent to reach the Goauld server (see  [agent/tunnels#egress-proxies]({{< ref "02-agent/01-tunnels" >}}#egress-proxies)), but this behavior can be overridden using:

- `--http-custom-proxy`: override the system proxy
- `--http-proxy-username`: Username to authenticate on the proxy
- `--http-proxy-password`: Password to authenticate on the proxy
- `--http-proxy-domain`: Domain to authenticate on the proxy
- `--http`/`--no-http`: Enable/Disable the HTTP proxy 

> [!NOTE]
> This proxy is automatically enabled if the HTTP proxy with NTLM/Kerberos authentication is enabled, or if the SOCKS proxy is configured to use the HTTP proxy (or transitively, the MITM HTTP proxy)

## HTTP proxy with NTLM/Kerberos authentication

Some applications require NTLM/Kerberos authentication.

Given that the goal of the tool is to expose network access to the client as if the request were performed from the host, this proxy aims to transparently respond to NTLM/Kerberos authentication requests.

> [!NOTE]
> Because NTLM/Kerberos application authentication requires intercepting traffic (MITM) in order to inject the appropriate headers, this feature is implemented in a dedicated proxy.

This proxy uses the previously described HTTP proxy as its upstream proxy, in order to handle the system proxy and the proxy authentication if required.

> [!WARNING]
> This proxy is only available on Windows.



### Flags

- `--mitm-http`/`--no-mitm-http`: Enable/Disable the MITM HTTP proxy 

> [!NOTE]
> This proxy is automatically enabled if the SOCKS proxy is configured to use the MITM HTTP proxy

## SOCKS proxy

The SOCKS proxy can be configured to use different HTTP upstream proxies:

- `http`: agent HTTP upstream proxy
- `mitm`: agent HTTP with NTLM/Kerberos upstream proxy
- `system`: the default HTTP proxy configured on the host (the proxy used by the agent to access the Goauld server)
- `custom`: the custom proxy configured in the `--socks-custom-proxy` flag
- `none`: no upstream proxy

### Flags

- `--socks-custom-proxy`: Custom upstream HTTP proxy used within the SOCKS proxy
- `--socks-proxy`: Configure the upstream HTTP proxy to use (none|system|http|mitm|custom)
- `--socks-proxy-username`: Username for the SOCKS upstream proxy
- `--socks-proxy-password`: Password for the SOCKS upstream proxy
- `--socks-proxy-domain`: Domain for the SOCKS upstream proxy
- `--socks`/`--no-socks`: Enable/Disable the SOCKS proxy
