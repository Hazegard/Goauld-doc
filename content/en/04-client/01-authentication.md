---
title: Client authentication
description: Goauld client authentication
weight: 1
---

## User authentication

To interact with the server, users need an access token.

Provide it via:
- CLI flag: `--access-token`
- Configuration file
- Environment variable
- Compile-time value

See [general/variables]({{< ref "01-general/03-variables" >}}) for detailed precedence and configuration options.


### Flag

- `--access-token`

```yaml
access-token: XXXXX
```



## Agent authentication

All interaction with the agents (including SSH, SCP/Rsync, Kill/Reset/Delete commands, Clipboard operations, etc.) requires the agent's static password, set via the `-P`/`--password` flag. See [agent/password management]({{< ref "02-agent/05-password_management" >}}) for details.

## Admin authentication

See [server/access control]({{< ref "03-server/03-access_control" >}})

The `--admin-token` restricts access to administrative endpoints (`/admin/`).

### Flag

- `--admin-token`

```yaml
admin-token: XXXXX
```

## Agent binaries authentication

See [server/agent downloading]({{< ref "03-server/05-agent_downloading" >}})


