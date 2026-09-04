---
title: Forward
description: Goauld client port, Unix socket, and named pipe forwarding
weight: 20
---

## Forward TCP ports, Unix sockets, or named pipes

`tealc forward [AGENT_NAME]` forwards traffic between the client and an agent, using the same `-L`/`-R` syntax as OpenSSH. You can also manage forwards interactively in the TUI using `[ctrl+f]` — see [client/tui]({{< ref "04-client/02-tui" >}}#port-forwarding-menu) for details.

```bash
tealc forward [AGENT_NAME] -L [bind_addr:]port:host:hostport
tealc forward [AGENT_NAME] -R [bind_addr:]port:host:hostport
```

- `-L` (local forward): the client listens, and forwards each accepted connection to the target through the agent.
- `-R` (remote forward): the agent listens, and forwards each accepted connection back to the client, which reaches the target from its own side. This is the point of a reverse forward: it works even when the agent itself cannot reach the target.

Either flag can be repeated to set up several forwards from a single command, and either side of a forward, the bind address or the target, can be a TCP address, a Unix socket path, or (on Windows) a named pipe path, in any combination.

## Examples

```bash
# TCP: client listens on 127.0.0.1:8080, agent connects to 127.0.0.1:2375
tealc forward myagent -L 8080:127.0.0.1:2375

# Unix socket bind, TCP target
tealc forward myagent -L /tmp/docker.sock:127.0.0.1:2375

# TCP bind, Unix socket target (no port on a socket/pipe address)
tealc forward myagent -L 8080:/var/run/docker.sock

# Named pipe on a Windows client, Unix socket on the agent
tealc forward myagent -L \\.\pipe\docker:/var/run/docker.sock

# Reverse forward: the agent listens, the client reaches the target
tealc forward myagent -R /var/run/docker.sock:127.0.0.1:2375

# Multiple forwards in one command
tealc forward myagent \
  -L 8080:127.0.0.1:9000 \
  -L /tmp/db.sock:127.0.0.1:5432 \
  -R 3000:localhost:3000
```

## Address format

`[bind_addr:]port:host:hostport`, matching OpenSSH:

- Omitting `bind_addr` defaults it to `127.0.0.1`.
- A bind or target address starting with `/` is treated as a Unix socket path, and a socket/pipe address never carries a port.
- A bind or target address starting with `\\.` is treated as a Windows named pipe path.

Two `-L` forwards in the same command cannot bind the same local address, whether TCP, Unix socket, or named pipe. The command rejects that conflict before opening the SSH connection.

## Platform support

- TCP and Unix sockets are forwarded over the real SSH wire protocols: `direct-tcpip`/`tcpip-forward` for TCP, and OpenSSH's own streamlocal extension for Unix sockets. A modern Windows client (Windows 10 1803 or later) can use Unix socket addresses too, since Windows itself now supports them.
- Windows named pipes have no equivalent in the SSH or OpenSSH protocols, so forwarding one uses a protocol of `tealc`'s own. This only comes into play when a forward actually names a pipe; TCP and Unix socket forwards are unaffected.
- A forward's address types are not validated against the agent's OS ahead of time. If a spec asks the agent to do something its OS cannot (for example, listening on a named pipe from a Linux agent), the agent rejects the request and the error is reported back, the same way an incompatible OpenSSH forward would fail.

> [!NOTE]
> If several forwards are given and one of them fails, the others keep running. The command only reports an overall failure if every forward failed.
