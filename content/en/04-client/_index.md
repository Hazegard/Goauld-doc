---
title: Client features
description: Goauld client (tealc), the operator's tool for connecting to and interacting with agents
weight: 4
---

The client, `tealc`, is the operator-facing tool used to connect to agents through the server: opening SSH sessions, forwarding proxies, transferring files, and managing agents.

> [!NOTE]
> On every invocation, the client checks the server's version and logs a warning if the server is running a newer version than the client. This is only a compatibility notice; it does not block the command from running.

This section covers the client's features:

- [Client authentication]({{< ref "04-client/01-authentication" >}}): how the client authenticates to the server and agents.
- [TUI]({{< ref "04-client/02-tui" >}}): the text-based interface for monitoring and managing connected agents.
- [SSH]({{< ref "04-client/03-ssh" >}}): connecting to an agent's shell.
- [Proxies]({{< ref "04-client/04-proxies" >}}): exposing an agent's proxies locally.
- [SCP]({{< ref "04-client/05-scp" >}}): transferring files with SCP, Rsync and Rclone.
- [Jump]({{< ref "04-client/06-jump" >}}): jumping to a target through an agent.
- [Clipboard]({{< ref "04-client/07-clipboard" >}}): sharing the clipboard with an agent.
- [Shell logging]({{< ref "04-client/08-shell-logging" >}}): recording SSH sessions to a log file.
- [Audit mode]({{< ref "04-client/09-audit-mode" >}}): redacting sensitive information in the TUI.
- [ControlMaster]({{< ref "04-client/10-controlmaster" >}}): reusing SSH connections.
- [WireGuard]({{< ref "04-client/11-wireguard" >}}): forwarding an agent's WireGuard interface.
- [Compile agent]({{< ref "04-client/12-compilation" >}}): compiling new agents from the client.
- [Admin features]({{< ref "04-client/13-admin" >}}): administrative operations available to the client.
- [Password]({{< ref "04-client/14-password" >}}): managing agent static passwords.
- [Embed server]({{< ref "04-client/15-embed_server" >}}): embedding server connection details in a compiled agent.
- [Agent binding]({{< ref "04-client/16-agent_binding" >}}): binding an agent to specific hosts.
- [Socks]({{< ref "04-client/17-socks" >}}): SOCKS proxy usage from the client.
- [VS Code]({{< ref "04-client/18-vscode" >}}): launching VS Code sessions against an agent.
- [Logs]({{< ref "04-client/19-logs" >}}): retrieving recorded logs from an agent.
- [Forward]({{< ref "04-client/20-forward" >}}): forwarding TCP ports, Unix sockets, or named pipes.
- [Shell completion]({{< ref "04-client/21-completion" >}}): generating and installing tealc completion for zsh, bash, and fish.
</content>
