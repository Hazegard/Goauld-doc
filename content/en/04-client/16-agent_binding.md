---
title: Agent binding
description:
weight: 16
---

When the agent is in bind mode, it exposes a port on which the client connects to. (see [agent/tunnels]({{< ref "02-agent/01-tunnels" >}}#agent-binding))

## Flags

- `--kill` whether to kill the agent on disconnection
- `[AGENT_ADDR]`: the agent to bind to, format: `[IP]:[PORT]`
- `--age-privkey`: age private key to use for the embedded server (overrides the compiled-in one).
- `--http-listen-addr`, `--https-listen-addr`, `--ssh-listen-addr`, `--dns-listen-addr`, `--quic-listen-addr`: override the listen addresses for the embedded server's respective services, matching their [server/services]({{< ref "03-server/01-services" >}}) counterparts.
