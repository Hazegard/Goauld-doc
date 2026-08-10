---
title: Embed server
description:
weight: 15
---

The client can enter server mode, allowing it to reproduce the `nc -lvp` command.

```bash
tealc embed-server
```

> [!NOTE]
> The deployed agent must embed or be passed an appropriate Age public key


> [!NOTE]
> The agent automatically kills itself after the shell session ends

## Flags

- `--age-privkey`: age private key to use for the embedded server (overrides the compiled-in one).
- `--http-listen-addr`, `--https-listen-addr`, `--ssh-listen-addr`, `--dns-listen-addr`, `--quic-listen-addr`: override the listen addresses for the embedded server's respective services, matching their [server/services]({{< ref "03-server/01-services" >}}) counterparts.

## Demo

<video width="90%" controls autoplay muted>
    <source src="embed-server.webm" type="video/webm">
    Your browser does not support the video tag.
</video>
