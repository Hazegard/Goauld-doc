---
title: Client authentication
description:
weight: 1
---

## User authentication

To interact with the server, users need an access token.

The access token can be provided through all means discussed here: [general/variables]({{< ref "01-general/03-variables" >}})


### Flag

- `--access-token`

```yaml
access-token: XXXXX
```



## Agent authentication

All interaction with the agents (including SSH, SCP/Rsync, Kill/Reset/Delete commands , Clipboard operations, etc.) requires the agent password. (see [agent/password management]({{< ref "02-agent/05-password_management" >}}))

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


