---
title: Clipboard
description:
weight: 7
---


Access to the agent clipboard has bee implemented to quickly share small text between the client and the agent.

## Retrieve the agent clipboard
The content will be printed to STDOUT
```bash
tealc clip get [AGENT]
```



## Set the agent clipboard

```bash
tealc clip set [AGENT] [CONTENT]
```

> [!WARNING]
> This feature is recommended for small content only. For larger content, use SCP or Rsync instead. (see [client/scp]({{< ref "04-client/05-scp" >}}))
