---
title: Server features
description: "Goauld server component: services, deployment, access control, and more"
weight: 3
---

The server is the publicly reachable component of Goauld. It exposes the listening services agents connect through, brokers access between operators and agents, and persists agent state across restarts.

This section covers:

- [Listening services]({{< ref "03-server/01-services" >}}): the transports the server exposes and how agents tunnel SSH over them.
- [Deployment]({{< ref "03-server/02-deployment" >}}): example configuration for running the server, including with Docker.
- [Access control]({{< ref "03-server/03-access_control" >}}): IP allowlisting and access/admin tokens for the server's endpoints.
- [Healthcheck]({{< ref "03-server/04-healthcheck" >}}): scripts and tools to verify tunnels are functioning.
- [Agent downloading]({{< ref "03-server/05-agent_downloading" >}}): serving precompiled agent binaries for download.
- [Database]({{< ref "03-server/06-database" >}}): how the server persists agent identities and state.
