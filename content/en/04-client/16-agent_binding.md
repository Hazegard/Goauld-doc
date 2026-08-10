---
title: Agent binding
description: Goauld client agent binding
weight: 16
---

When the agent is in bind mode, the client connects directly to the port exposed by the agent (see [agent/tunnels]({{< ref "02-agent/01-tunnels" >}}#agent-binding)).

To do this, `tealc bind` starts a full local server instance (the same embedded server used by `tealc embed-server`, see [client/embed server]({{< ref "04-client/15-embed_server" >}})) to accept the agent's connection.

```bash
tealc bind <AGENT_ADDR>
```

> [!NOTE]
> Only the first agent to connect is used to open an SSH session. Any agent that connects afterward is terminated immediately; this happens regardless of the `--kill` flag. `--kill` only controls whether the agent actually used for the session is terminated once that session ends.

## Flags

- `--kill` whether to kill the agent on disconnection
- `<AGENT_ADDR>`: the agent to bind to, format: `[IP]:[PORT]`
- `--age-privkey`: age private key for the embedded server. `tealc` requires this key to be provided via one of these methods:
  - `--age-privkey` flag
  - Configuration file entry
  - Environment variable
  - Compile-time embedding via `CLIENT__AGE_PRIVKEY` (see [client/compilation]({{< ref "04-client/12-compilation" >}}))
- `--http-listen-addr`, `--https-listen-addr`, `--ssh-listen-addr`, `--dns-listen-addr`, `--quic-listen-addr`: override the listen addresses for the embedded server's respective services, matching their [server/services]({{< ref "03-server/01-services" >}}) counterparts.
