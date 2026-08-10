---
title: Jump
description:
weight: 6
---


In some assessments, the agent is deployed on a bounce (jump) machine, from which we can access another machine, from which the assessment is performed.

In order to simplify the access to the assessment machine, a wrapper has been implemented in the client:

```bash
tealc jump --print [AGENT_NAME] [REMOTE_SERVER] (-i ./id_ed25519)
```

> [!NOTE]
> This command is simply:
> ```bash
> ssh -oProxyCommand="tealc ssh [AGENT_NAME] -W %h:%p [REMOTE_SERVER]" (-i ./id_ed25519)
> ```

It wraps the underlying SSH ProxyCommand, providing convenience for repeated jump connections.

### Flags

- `--scp`: route the jump connection through `scp` instead of `ssh`
- `--log`: record the session to a log file
- `--[no-]print`: print the generated command instead of running it