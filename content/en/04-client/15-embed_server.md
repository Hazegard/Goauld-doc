---
title: Embed server
description: Goauld client embedded server mode
weight: 15
---

The client can enter server mode, allowing it to emulate a listening server (similar to `nc -lvp`).

```bash
tealc embed-server
```

> [!NOTE]
> The deployed agent must have an appropriate age public key (embedded at compile-time or provided at runtime).


> [!NOTE]
> The agent automatically kills itself after the shell session ends

## Flags

- `--age-privkey`: age private key for the embedded server. `tealc` requires this key to be provided via one of these methods:
  - `--age-privkey` flag
  - Configuration file entry
  - Environment variable
  - Compile-time embedding via `CLIENT__AGE_PRIVKEY` (see [client/compilation]({{< ref "04-client/12-compilation" >}}))
- `--http-listen-addr`, `--https-listen-addr`, `--dns-listen-addr`, `--quic-listen-addr`: override the listen addresses for the embedded server's respective services, matching their [server/services]({{< ref "03-server/01-services" >}}) counterparts.

> [!NOTE]
> `--ssh-listen-addr` exists as a flag but has no effect here: `tealc embed-server` never binds a raw SSH TCP listener. The connecting agent reaches sshd in-process regardless of transport, and the operator's own SSH connection is always routed through the embedded server's `/ssh-ws/` WebSocket endpoint instead of a `-p<port>` TCP dial (see [Connecting over WebSocket]({{< ref "04-client/03-ssh" >}}#connecting-over-websocket) in client/SSH).

## Demo

<video width="90%" controls autoplay muted>
    <source src="embed-server.webm" type="video/webm">
    Your browser does not support the video tag.
</video>
