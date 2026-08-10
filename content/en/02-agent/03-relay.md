---
title: Relay
description: Goauld agent relaying
weight: 3
---


Agent B can relay connections from Agent A when: Agent A cannot reach the server directly, but can reach Agent B, and Agent B can reach the server.


## Configure an agent as a relay

- `--relay`: Enable relay mode on the agent
- `--relay-port`: Port to listen on (default: `0` = random port)

> [!NOTE]
> By default, the relay listens on all interfaces using a randomly assigned port.

To discover the assigned port, check the agent logs:
```log
INF agent/agent.go:522 > Relay listening on port Port=57129
```

Or view it in the TUI by pressing `+` to expand agent details.

To use a fixed port instead of a random one, set `--relay-port` to a specific value.



## Set an agent as upstream relay

- `--relay-addr=[IP_AGENT]:[PORT]`: Upstream relay agent address and port

> [!NOTE]
> Setting this automatically configures the agent to use SSH over WebSocket, as relay communication requires WebSocket transport.

## How the relay works

The relay exposes a local HTTP web server that handles two types of connections:

- `Socket.IO connections`: The relay acts as a protocol bridge by accepting and re-emitting Socket.IO traffic. This allows it to handle multiple transport types (WebSocket, HTTP polling, DNS-tunneled WebSocket).
- `SSH connections`: The relay handles SSH over WebSocket connections locally. These connections are decapsulated and forwarded to the Goauld server using the tunnel of the upstream relay agent.