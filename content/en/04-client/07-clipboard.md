---
title: Clipboard
description: Goauld client clipboard access
weight: 7
---


The agent clipboard can be accessed remotely to quickly share small text between the client and the agent.

## Retrieve the agent clipboard
The content will be printed to STDOUT
```bash
tealc clip get [AGENT]
```



## Set the agent clipboard

```bash
tealc clip set [AGENT] [CONTENT]
```

> [!NOTE]
> When SSH ControlMaster is enabled (see [client/controlmaster]({{< ref "04-client/10-controlmaster" >}})), these commands reuse an existing master connection if available, avoiding the password prompt. If no master connection is open for the agent, a new connection is automatically created, which will also establish the ControlMaster socket for future use.

> [!WARNING]
> This feature is recommended for small content only. For larger content, use SCP or Rsync instead. (see [client/scp]({{< ref "04-client/05-scp" >}}))
