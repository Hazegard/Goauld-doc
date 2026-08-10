---
title: Jump
description: Goauld client jump connections
weight: 6
---


When the agent is deployed on an intermediate (jump) machine, use this wrapper to simplify access to the assessment target:

```bash
tealc jump --print [AGENT_NAME] [REMOTE_SERVER] (-i ./id_ed25519)
```

> [!NOTE]
> This command is roughly equivalent to:
> ```bash
> ssh -oProxyCommand="tealc ssh [AGENT_NAME] -W %h:%p" [REMOTE_SERVER] (-i ./id_ed25519)
> ```
> When SSH ControlMaster is enabled (see [client/controlmaster]({{< ref "04-client/10-controlmaster" >}})), `-M` is also added to the inner `tealc ssh` call.

It wraps the underlying SSH ProxyCommand, providing convenience for repeated jump connections.

With `--scp`, the additional arguments must use `scp`-style `host:path` targets instead of a bare remote host:

```bash
tealc jump --scp --print [AGENT_NAME] [REMOTE_SERVER]:/PATH/TO/TARGET/FILE /PATH/TO/SOURCE/FILE
```

## Flags

- `--scp`: route the jump connection through `scp` instead of `ssh`
- `--log`: record the session to a log file
- `--print`: print the generated command instead of running it