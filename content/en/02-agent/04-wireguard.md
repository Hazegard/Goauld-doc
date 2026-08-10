---
title: WireGuard
description: Goauld agent WireGuard
weight: 4
---

The agent exposes a virtual WireGuard interface, allowing TCP, UDP, and ICMP (ping) traffic without relying on a SOCKS proxy.


The virtual WireGuard interface uses the gVisor network stack ([https://github.com/google/gvisor/tree/go](https://github.com/google/gvisor/tree/go)).

The implementation works as follows:
1. The agent exposes a WireGuard server port on the host.
2. The agent forwards the WireGuard port to the server using UDP-over-TCP encapsulation to traverse the existing agent tunnel.
3. The client forwards the WireGuard port exposed on the server to the local machine.
4. The client decapsulates the UDP-over-TCP traffic to expose the WireGuard port.
5. The WireGuard client on the operator machine connects to the agent's virtual WireGuard interface.

> [!NOTE]
> This implementation uses UDP-over-TCP encapsulation, which reduces performance.
> However, this architecture was chosen because the server does not expose a WireGuard server common to all connected agents, which could result in unauthorized access between agents.


> [!WARNING]
> The agent runs without privileges. Consequently it cannot forward raw packets (such as ICMP packets, or Nmap SYN scans).
> 
> `nmap` must be run using `--unprivileged` or `-PE`.

## Flags

The virtual WireGuard interface is not enabled by default.

- `--wg`: Enable the WireGuard interface


See  [client/WireGuard]({{< ref "04-client/11-wireguard" >}})