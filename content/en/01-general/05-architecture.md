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

![Goauld architecture](Goauld.png)
