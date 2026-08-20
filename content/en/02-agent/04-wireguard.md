---
title: WireGuard
description: Goauld agent WireGuard
weight: 4
---

The agent exposes a virtual WireGuard interface, allowing TCP, UDP, and ICMP (ping) traffic without relying on a SOCKS proxy.


The virtual WireGuard interface uses the gVisor network stack ([https://github.com/google/gvisor/tree/go](https://github.com/google/gvisor/tree/go)).

Each agent gets its own private WireGuard endpoint, tunneled through the existing agent connection: enable it with `--wg-enabled`, then connect a WireGuard client to the port exposed on the client side (see [client/wireguard]({{< ref "04-client/11-wireguard" >}})).

> [!NOTE]
> The tunnel uses UDP-over-TCP encapsulation, which reduces performance compared to native WireGuard but enables operation over custom transports.


> [!WARNING]
> The agent runs without privileges and cannot forward raw packets (required for ICMP ping or Nmap SYN scans).
>
> When using nmap, choose one of these alternatives:
> - `nmap --unprivileged`: Disables raw packet features, uses TCP connect scans instead
> - `nmap -PE`: Uses ICMP echo ping instead of SYN probes for host discovery

## Flags

The virtual WireGuard interface is not enabled by default.

- `--wg-enabled`: Enable the WireGuard interface
- `--wg-port`: the remote port the WireGuard interface binds to on the server side (default: `0`, meaning a random port is chosen).


See  [client/wireguard]({{< ref "04-client/11-wireguard" >}})