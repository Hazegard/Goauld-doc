---
title: Password
description: Goauld client agent password retrieval
weight: 14
---

> [!NOTE]
> This command is mostly internal. It is documented in case you need to manually connect to an agent via SSH without using `tealc`.

Manual usage:

```bash
tealc pass --agent [AGENT_NAME] --type [otp|agent] [ARGS]
```

## Flags

- `--agent`: the agent to retrieve the password for. If omitted, the first positional argument in `ARGS` is used instead.
- `--type`: password type to retrieve:
  - `otp`: one-time proxy password for `ProxyCommand` setup
  - `agent`: the agent's static password (described in [agent/password management]({{< ref "02-agent/05-password_management" >}}))
  - Omitted: prints both, labeled `OTP:` and `Agent:`

## How it's actually invoked as `SSH_ASKPASS`

This is the command `tealc ssh` and `tealc jump` set as `SSH_ASKPASS` (see the example SSH command in [client/ssh]({{< ref "04-client/03-ssh" >}})). In that context, ssh does not call it with `--agent`/`--type` flags: it sets `TEALC_AGENT` and `TEALC_TYPE` as environment variables (along with `TEALC_SERVER`, `TEALC_SSH_SERVER`, `TEALC_ACCESS_TOKEN`, etc.), and invokes the binary with a single positional argument: the literal prompt text ssh itself would normally show (e.g. `user@host's password: `).

That prompt text is matched against the target agent's known `agent@host` and `agent@id` strings to decide which secret to print, falling back to `--type`/`TEALC_TYPE` only if it doesn't match either. To manually reproduce this - for connecting via a plain `ssh` without `tealc` - set those environment variables yourself and point `SSH_ASKPASS` (with `SSH_ASKPASS_REQUIRE=force`) at the `tealc` binary, rather than passing `--agent`/`--type` on the command line.

