---
title: General information
description: Goauld general information
weight: 1
---

Goauld is a post-exploitation and remote access tool built for engagements where regular remote-access tooling struggles: restricted corporate networks, authenticated egress proxies, and environments where only a narrow set of protocols is allowed to leave the network.

It is composed of three components:

- **Server**: the publicly reachable component. It exposes an SSH server both directly and through a range of tunneling transports (TLS, WebSocket, HTTP, DNS, and QUIC), and brokers access between operators and agents.
- **Agent**: deployed on the target machine. It embeds an SSH server along with SOCKS and HTTP proxies, and establishes an outbound connection back to the server. No inbound access to the target is required.
- **Client (tealc)**: the operator's tool. It connects to the server to interact with agents: opening SSH sessions, forwarding proxies, transferring files, and compiling new agents.

All traffic between an agent and the server is encapsulated inside a single outbound SSH tunnel, whichever transport carries it, keeping the underlying access model consistent regardless of network restrictions.

This section covers the fundamentals shared across all three components:

- [Quick start]({{< ref "01-general/01-quick_start" >}}): get a server, agent, and client running end to end.
- [Compilation]({{< ref "01-general/02-compilation" >}}): build each component from source.
- [Variables]({{< ref "01-general/03-variables" >}}): how configuration values are sourced and prioritized.
- [Configuration file]({{< ref "01-general/04-configuration_file" >}}): where each component looks for its config file.
- [Architecture]({{< ref "01-general/05-architecture" >}}): a diagram of how the pieces fit together.
- [Minimal HTTP(S)-only setup]({{< ref "01-general/06-minimal-http-setup" >}}): run the deployment with only HTTP/HTTPS exposed.
