---
title: Password
description: Goauld client agent password retrieval
weight: 14
---

> [!NOTE]
> This command is mostly internal. It is documented in case you need to manually connect to an agent via SSH without using `tealc`.

```bash
tealc pass --agent [AGENT_NAME] --type [otp|agent] [ARGS]
```

## Flags

- `--agent`: the agent to retrieve the password for.
- `--type`: password type to retrieve:
  - `otp`: one-time proxy password for `ProxyCommand` setup
  - `agent`: the agent's static password (described in [agent/password management]({{< ref "02-agent/05-password_management" >}}))
  - Omitted: prints both, labeled `OTP:` and `Agent:`

This is the command invoked internally as `SSH_ASKPASS` by `tealc ssh` and `tealc jump` (see the example SSH command in [client/ssh]({{< ref "04-client/03-ssh" >}})). It is not typically run directly.

