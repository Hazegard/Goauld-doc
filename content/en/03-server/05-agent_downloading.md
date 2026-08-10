---
title: Agent downloading
description: Downloading agent binaries from the server
weight: 5
---

To distribute precompiled agents to targets, serve them from the `/binaries/` endpoint via https://[SERVER]/binaries/.

## Configuration

By default (`binaries-basic-auth` unset), `/binaries/` is served with no authentication. To protect it, configure a username/password:

```yaml
# HTTP Basic Auth credentials required to access the binaries endpoint.
binaries-basic-auth: user:password
```

Once set, requests without valid Basic Auth credentials for that user/password are rejected with a 401.

> [!NOTE]
> `binaries-path-location` must be configured for `/binaries/` to serve files. When `binaries-path-location` is unset, responses depend on authentication:
> 
> | Scenario | Response |
> |----------|----------|
> | Basic Auth configured and credentials fail | `401` (Unauthorized) |
> | Basic Auth configured and credentials pass | `404` (Not Found) |
> | No Basic Auth configured | `404` (Not Found) |

