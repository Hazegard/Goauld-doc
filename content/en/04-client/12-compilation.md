---
title: Compile agent
description:
weight: 12
---

The client can recompile agents for different platforms and architectures.


```bash
tealc compile (--id [ID]) (-O [OS]) (-A [ARCH])
```

The `ARCH` flag can be:
- `amd64`
- `arm64`
- `arm`
- `386`

The `OS` flag can be:
- `darwin`
- `linux`
- `windows`


## Compile the agent with custom default values

1. Generate the configuration file:
```bash
tealc compile --drop-env > ./env.txt
```

This configuration file can then be modified to set custom defaults before recompilation.

```bash
tealc compile --env [/PATH/TO/ENV]
```