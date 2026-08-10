---
title: SOCKS
description: Goauld client SOCKS/HTTP-only proxy shortcut
weight: 17
---

`tealc socks` is a shortcut around `tealc ssh` that forwards only the SOCKS and HTTP proxies (no MITM HTTP, no WireGuard), without opening an interactive shell by default.

```bash
tealc socks [AGENT_NAME]
```

Like `tealc ssh`, `tealc socks` also accepts additional arguments, but they only reach the underlying SSH command when `--ssh` is active. By default `--ssh` is disabled: only the connection to the Goauld server is made, to forward the proxy ports, so there is no SSH connection for the additional arguments to reach.

## Flags

- `--[no-]socks`: Forward the agent's SOCKS proxy to the local host (enabled by default).
- `--[no-]http`: Forward the agent's HTTP proxy to the local host (enabled by default).
- `--socks-port`: Local port to bind the SOCKS proxy (default: `1080`).
- `--http-port`: Local port to bind the HTTP proxy (default: `3128`).
- `--[no-]ssh`: Also connect to the agent's SSH service (disabled by default). Additional arguments are only forwarded to the underlying SSH command when this is enabled.
- `--[no-]print`: Print the generated SSH command instead of executing it.
- `--proxy`: Use direct STDIN/STDOUT mode for ProxyCommand compatibility. This disables SOCKS and HTTP forwarding entirely (and takes priority over `--ssh`), leaving a plain SSH tunnel with none of `tealc socks`'s usual forwarding.
