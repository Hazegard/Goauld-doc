---
title: Agent features
description: Goauld agent features
weight: 2
---

The agent is the component deployed on the target machine. This section covers its features: the transports used to reach the server, the proxies and pivoting services it exposes, password handling, the killswitch failsafe, the connection sequence, and working-hours scheduling.

- [Tunneling]({{< ref "02-agent/01-tunnels" >}}): the transports the agent uses to reach the server, and how it falls back between them.
- [Exposed proxies]({{< ref "02-agent/02-proxies" >}}): the SOCKS, HTTP, and MITM HTTP proxies exposed by the agent.
- [Relay]({{< ref "02-agent/03-relay" >}}): how an agent can relay traffic for another agent that cannot reach the server directly.
- [WireGuard]({{< ref "02-agent/04-wireguard" >}}): the virtual WireGuard interface used for network pivoting.
- [Password management]({{< ref "02-agent/05-password_management" >}}): how the local agent password is generated, set, and disabled.
- [Killswitch]({{< ref "02-agent/06-killswitch" >}}): the failsafe that shuts down the agent after a set number of days.
- [Connection flow]({{< ref "02-agent/07-connection_flow" >}}): how the control and data sockets are established and maintained.
- [Working days]({{< ref "02-agent/08-working_days" >}}): scheduling network activity to specific hours and days.
