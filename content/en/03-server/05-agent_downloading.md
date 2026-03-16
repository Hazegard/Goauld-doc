---
title: Agent Drop
description: Agent dropping
weight: 5
---

In order to drop precompiled agents, a basicauth protected dirlisting is available through https://[SERVER]/binaries/.

To access this dirlisting, a custom password should be configured.

```yaml
# HTTP Basic Auth credentials required to access the binaries endpoint.
binaries-basic-auth: user:password
```

