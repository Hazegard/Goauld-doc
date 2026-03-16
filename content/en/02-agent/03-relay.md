---
title: Relay
description: Goauld agent relaying
weight: 3
---


If an agent `A` cannot reach the Goauld server, but can reach another Goauld agent `B` that can reach the server, then agent `B` can be configured as a relay.


## Configure an agent as a relay

- `--relay`: Enable relay mode on the agent


> [!NOTE]
> The agent listens on all interfaces using a randomly assigned port. This port is logged in the agent logs:
>```log
>INF agent/agent.go:468 > Relay listening on port Port=57129
>```
>Or in the TUI (Press `+` to view details about the agent)



## Set an agent as upstream relay

- `--relay-addr=[IP_AGENT]:[PORT]`: Set the upstream relay agent that this agent should connect to.

This automatically configures the agent to use SSH over WebSocket, as the relay communication is performed exclusively over WebSocket.

## How the relay works

The relay exposes a local HTTP web server that handles two types of connections:
- `Socket.IO connections`: The relay re-emits all requests and responses. This is required to perform a protocol break, allowing it to handle all Socket.IO transport types, including WebSocket, HTTP polling, and WebSockets tunneled over DNS.
- `SSH connections`: The relay handles SSH over WebSocket connections locally. These connections are decapsulated and forwarded to the Goauld server using the tunnel of the upstream relay agent.