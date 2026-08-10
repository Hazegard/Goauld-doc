---
title: Agent Drop
description: Agent dropping
weight: 5
---

In order to drop precompiled agents, a dirlisting is available through https://[SERVER]/binaries/.

> [!WARNING]
> Verified current behavior is the opposite of what the flag name suggests:
> - If `binaries-basic-auth` is **left unset**, `/binaries/` requires HTTP Basic Auth on every request but no valid credentials exist, so the endpoint is effectively fully inaccessible (fails closed).
> - If `binaries-basic-auth` is **explicitly configured** (as below), the Basic Auth check is skipped entirely and `/binaries/` is served **without any authentication**.
>
> In other words, configuring a password currently removes access control instead of enabling it. Until this is fixed, treat `/binaries/` as unauthenticated once `binaries-basic-auth` is set, and restrict exposure through other means (`--allowed-ips`, network-level controls, etc.) if that matters for your deployment.

```yaml
# HTTP Basic Auth credentials required to access the binaries endpoint.
binaries-basic-auth: user:password
```

