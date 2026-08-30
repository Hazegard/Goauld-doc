---
title: Architecture
description: Goauld architecture overview
weight: 5
---

The diagram below shows how the three Goauld components fit together: the **server**, deployed on a publicly reachable machine; the **agent**, deployed on the target machine; and the **client** (`tealc`), used by the operator to interact with the deployment.

## Agent Connection

The agent never accepts inbound connections. Instead, it establishes an outbound-only connection back to the server. This allows agents to reach the server even from restricted networks where only a narrow set of outbound protocols is allowed.

## Transport Flexibility

The outbound connection is carried over one of several tunneling transports: TLS, WebSocket, HTTP, DNS, or QUIC. All traffic uses a single outbound SSH tunnel, regardless of transport choice. This keeps the access model consistent to the server, independent of agent-side network conditions.

## Server as Broker

On the server side, the SSH tunnel is where agents authenticate and register themselves. The server exposes an HTTP API that clients use to interact with agents: browsing connected agents, opening SSH sessions, forwarding proxies, transferring files, and compiling new agents.

The server acts as a broker: it terminates the agent's tunnel on one side and exposes a management interface to operators on the other, without either side needing to reach the other directly.

The default SSH listener exposed by the server is `:2222` (see [general/quick_start]({{< ref "01-general/01-quick_start" >}})).

## In-process routing

Beyond its core transport listeners (SSH, TLS, HTTP, DNS, QUIC, see [server/services]({{< ref "03-server/01-services" >}})), the server never opens a real listening socket per forwarded proxy or port. An agent's SOCKS/HTTP/MITM proxies and remote port forwards (see [agent/proxies]({{< ref "02-agent/02-proxies" >}})) are each identified by a virtual port scoped to that agent's own SSH connection, resolved entirely in-process when an operator's client asks to reach one. The same applies on the client side for `tealc bind` and `tealc embed-server` (see [client/agent binding]({{< ref "04-client/16-agent_binding" >}}) and [client/embed server]({{< ref "04-client/15-embed_server" >}})): the embedded server they run never binds a raw SSH TCP listener either, and reaches the connecting agent's sshd in-process instead. This keeps the number of sockets actually exposed on any host to the minimum needed for the outbound tunnel and the operator-facing API, regardless of how many proxies or forwards are active behind it.

![Goauld architecture](Goauld.png)
