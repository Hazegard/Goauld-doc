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



## Route an agent through a relay

An agent is routed through a relay with two flags:

- `--rssh-order=relay`: Route this agent's traffic through a relay agent
- `--server=[IP_AGENT]:[PORT]`: Address and port of the relay agent

The relay's address goes in `--server`: the same flag that points at the control HTTP server in a direct deployment. When `--rssh-order=relay` is set, the agent dials that address as a relay instead of as the Goauld server.

```bash
goauld --rssh-order=relay --server=10.0.0.5:57129
```

> [!NOTE]
> Relay communication requires WebSocket transport, so `--rssh-order=relay` configures both the control socket and the SSH tunnel to use WebSocket automatically. Pass `relay` on its own rather than combining it with other transports in the same `--rssh-order` list.

> [!WARNING]
> Earlier versions used a dedicated `--relay-addr` flag for this. It has been removed, use `--rssh-order=relay` together with `--server` as shown above.

## How the relay works

The relay exposes a local HTTP web server that handles two types of connections:

- `Socket.IO connections`: The relay acts as a protocol bridge by accepting and re-emitting Socket.IO traffic. This allows it to handle multiple transport types (WebSocket, HTTP polling, DNS-tunneled WebSocket).
- `SSH connections`: The relay handles SSH over WebSocket connections locally. These connections are decapsulated and forwarded to the Goauld server using the tunnel of the upstream relay agent.