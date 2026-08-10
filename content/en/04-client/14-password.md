---
title: Password
description:
weight: 14
---

> [!NOTE]
> This command is mostly an internal one.
> But it is still documented in case on manual connection to the agents through the server

```bash
tealc pass --agent [AGENT_NAME] --type [otp|agent] [ARGS]
```

- `--agent`: the agent to retrieve the password for.
- `--type`: `otp` retrieves the one-time proxy password used to set up the `ProxyCommand` step of a connection; `agent` retrieves the agent's own private password (the "Part 2" password described in [agent/password_management]({{< ref "02-agent/05-password_management" >}})).

This is the command invoked internally as `SSH_ASKPASS` by `tealc ssh` and `tealc jump` (see the example SSH command in [client/ssh]({{< ref "04-client/03-ssh" >}})) — it is not typically run directly.

