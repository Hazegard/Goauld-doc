---
title: Access Control
description: Goauld server access control
weight: 3
---

Certain components should be accessible only by authorized users.
- Admin endpoints
- Management endpoints
- SSH access from the client

## IP allowlisting

The server accepts a list of authorized IPs to restrict the access of
- The `/admin/` endpoints
- The `/manage/` endpoints
- SSH access from the client (using password authentication)

> [!WARNING]
> If the server runs in a docker environment, the deployment should ensure that the remote IP address is correctly forwarded to the server

### Flags

- `--allowed-ips=192.168.1.1,192.168.2.1`

> [!NOTE]
> Only exact IP addresses are matched; CIDR ranges (e.g. `192.168.2.0/24`) are accepted by the flag's validation but are not expanded/enforced as ranges — an entry like that will never actually match a real client IP. List every address individually.

> [!WARNING]
> If `--allowed-ips` is not set, allowlisting is fail-open: any IP is allowed. Set it explicitly to actually restrict access.

## Access token

### User access token


The user access token restricts:
- The `/manage/` endpoints


#### Flags

- `--access-token=token1,token2`


> [!NOTE]
> Multiple access tokens can be provided to segment user access.


### Admin access token

The admin access token restricts:
- The `/admin/` endpoints


#### Flags

- `--admin-token`

## Admin token embedding

Some `/manage/` endpoints (e.g. killing an agent via `POST /manage/agent/{id}/kill`) accept the admin token embedded in the same `Authorization` header used for the access token, separated by `:`:

```
Authorization: <access-token>:<admin-token>
```

Only the part before the first `:` is checked against the configured access tokens; the part after `:` is checked against the admin token to authorize the privileged action. Access tokens themselves may not contain `:`, since that character is reserved for this embedding scheme.

