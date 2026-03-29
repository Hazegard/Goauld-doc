---
title: Agent binding
description:
weight: 16
---

When the agent is in bind mode, it exposes a port on which the client connects to. (see [agent/tunnels]({{< ref "02-agent/01-tunnels" >}}#agent-binding))

## Flags

- `--kill` whether to kill the agent on disconnection
- `[AGENT_ADDR]`: the agent to bind to, format: `[IP]:[PORT]`
