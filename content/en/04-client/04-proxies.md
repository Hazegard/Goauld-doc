---
title: Proxies
description:
weight: 4
---


The client exposes the agent’s proxies locally on the host machine.

## Proxy SOCKS

Exposed by default on the port `1080`.

See [agent/proxies#socks-proxy]({{< ref "02-agent/02-proxies" >}}#socks-proxy) for details about the SOCKS proxy.


## Proxy HTTP

Exposed by default on the port `3128`.

See [agent/proxies#socks-proxy]({{< ref "02-agent/02-proxies" >}}#http-proxy) for details about the HTTP proxy.


## Proxy HTTP with NTLM/Kerberos application-level authentication

Exposed by default on the port `3129`.

See [agent/proxies#http-proxy-with-ntlmkerberos-authentication]({{< ref "02-agent/02-proxies" >}}#http-proxy-with-ntlmkerberos-authentication) for details about the HTTP MITM proxy.