---
title: WireGuard
description: Goauld client WireGuard TUN interface
weight: 11
---

See [agent/wireguard]({{< ref "02-agent/04-wireguard" >}})

A TUN interface is available to the client.

The TUN interface is a WireGuard VPN that allows TCP, UDP, and ICMP (ping only) traffic from the agent without relying on a SOCKS proxy.

> [!NOTE]
> The `--loopback` flag uses the special address range `240.0.0.0/8`, which is translated into `127.0.0.0/8` on the agent. This enables WireGuard clients to access the agent's loopback network (localhost services).

## How to use

1. Generate the WireGuard configuration and add it to the configuration file:

```bash
tealc wireguard generate
```

The content looks like:
```yaml
wg-public-key: yIBaM2WHj9gDcxRxIFtvKrxj6jrosjYsaP34i7BkJ2A=
wg-private-key: dPu5bpPpZLDi4t40iHDz+KCkIh0za32Jz+uxNdVth0Q=
wg-ip: 100.125.104.221
```

2. Start the WireGuard interface
```bash
tealc wireguard start [AGENT_NAME]
```

> [!NOTE]
> It requires `wg-quick` on Linux and macOS, and `wireguard` on Windows. On Linux and macOS, `tealc` runs `wg-quick` via `sudo`; make sure `sudo` is usable non-interactively (e.g. passwordless for this command), or the process will hang waiting for a password prompt. No elevation is used on Windows.

> [!NOTE]
> Configuration file naming: Agent names are truncated to 15 characters with "@" replaced by "_".

> [!NOTE]
> Configuration location: `[OS user cache dir]/tealc/[transformed-name].conf`
> - macOS example: `~/Library/Caches/tealc/user_host.conf`
> - File permissions: restricted to current user only

> [!NOTE]
> Using `--no-exec`: Shows the file path in manual `wg-quick`/`wireguard` commands for testing


## Flags

- `--range`: comma-separated IP ranges to route through the WireGuard VPN.
- `--auto-range`: automatically set the ranges from the agent.
- `--loopback`: use the loopback interface with the `240.0.0.0/8` address range.
- `--exec` / `--no-exec`: Execute WireGuard commands directly (default: execute). Use `--no-exec` to print commands instead of running them.
- `--port`: local port to bind the WireGuard proxy.
- `--wg-public-key`: WireGuard public key, passed directly instead of via the generated configuration file.
- `--wg-private-key`: WireGuard private key, passed directly instead of via the generated configuration file.
- `--wg-ip`: WireGuard tunnel IP address, passed directly instead of via the generated configuration file.