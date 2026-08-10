---
title: Documentation
linkTitle: Docs
menu: { main: { weight: 20 } }
type: "docs"
weight: 20

cascade:
- target:
    path: "/blog/**"
  type: "blog"
  # set to false to include a blog section in the section nav along with docs
  toc_root: true
- target:
    path: "/**"
    kind: "page"
  type: "docs"
- target:
    path: "/**"
    kind: "section"
  type: "docs"
- target:
    path: "/**"
    kind: "section"
  type: "home"
---

> Goauld is a post-exploitation and remote access tool designed for use in restricted environments.

During penetration tests, operators often face restrictive network environments:
- Working from a client-provided laptop behind VPNs
- Traversing authenticated egress proxies
- Operating under restrictive network controls

Additionally, gaining remote code execution on a system requires establishing a stable and fully interactive access channel.

Goauld provides a tunneling and access framework for remote agent interaction. It supports multiple transport protocols while maintaining a secure SSH-based architecture.

It is composed of three components:
- The server, exposing an SSH server both directly and through multiple tunneling transports
- The agent, which embeds an SSH server, SOCKS and HTTP proxies 
- The client (**tealc**), which allows access to agents and interaction with them


## Use cases

- Post-exploitation remote access
- Working from restricted corporate assessment laptops
- Bypassing authenticated proxies
- Pivoting through compromised hosts

![Goauld TUI showing connected agents](04-client/02-tui/TUI.png)

## Features

The main agent features are:
- Cross-platform (Windows, Linux and macOS)
- SSH encapsulation over multiple transports:
  - Direct SSH
  - TLS
  - QUIC
  - WebSocket
  - HTTP
  - DNS
- Support for egress proxies, with automatic NTLM/Kerberos authentication when required by the proxy itself
- Automatic NTLM/Kerberos application-level authentication when the targeted application requires it
- Exposes SOCKS and HTTP proxies, which can themselves route through an upstream HTTP proxy
- Full-blown interactive shell
- Copy files via integrated SCP, Rsync or Rclone
- Tun interface using an integrated virtual WireGuard embedded in the agent
- Agent binding: the agent can open a local port for the client to connect to
- Agent relaying

## Demo

<video width="90%" controls autoplay muted>
    <source src="Demo.webm" type="video/webm">
    Your browser does not support the video tag.
</video>


The global architecture is based on an outbound SSH tunnel from the agent to the server.
All communications are encapsulated within this tunnel in order to prioritize security and network segmentation.
