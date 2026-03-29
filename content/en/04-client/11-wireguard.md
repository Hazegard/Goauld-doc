---
title: Wireguard
description:
weight: 11
---

See [agent/wireguard]({{< ref "02-agent/04-wireguard" >}})
A TUN interface is available to the client.

The TUN interface is a WireGuard VPN that allows TCP, UDP, and ICMP (ping only) traffic from the agent without relying on a SOCKS proxy.

> [!NOTE]
> The special range `240.0.0.0/8` is translated into `127.0.0.0/8` on the agent, enabling access to the loopback range.

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

2. Start the WireGuard
```bash
tealc wireguard start
```

> [!NOTE]
> It requires `wg-tools` on linux and `wireguard` on windows.


## Flag