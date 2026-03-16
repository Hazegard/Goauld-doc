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
> Although this command is a simple :
> ```bash
> ssh -oProxyCommand="tealc ssh [AGENT_NAME] -W %h:%p [REMOTE_SERVER]" (-i ./id_ed25519)
> ```

Although it simply wraps the underlying SSH ProxyCommand, it provides convenience for repeated jump connections.