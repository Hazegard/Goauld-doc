---
title: Access control
description: Goauld server access control
weight: 3
---

Certain components should be accessible only by authorized users: the admin endpoints, the management endpoints, SSH access from the client, and SSH local port forwarding. Two independent mechanisms restrict this access, IP allowlisting and access tokens, described below.

## IP allowlisting

The server accepts a list of authorized IPs to restrict access to:
- The `/admin/` endpoints
- The `/manage/` endpoints
- SSH access from the client (using password authentication)
- SSH local port forwarding

> [!WARNING]
> If the server runs in a Docker environment, the deployment should ensure that the remote IP address is correctly forwarded to the server. For the `/admin/` and `/manage/` HTTP endpoints specifically, this can be done with `--trusted-proxies` if a reverse proxy sits in front of them (see [Reverse proxy support](#reverse-proxy-support) below). There is no equivalent for SSH access: see the note in that section.

### Flags

- `--allowed-ips=192.168.1.1,192.168.2.0/24`

Both individual IP addresses and CIDR ranges are matched. To allow all IPv4 addresses, set `--allowed-ips=0.0.0.0/0`; to also allow all IPv6 addresses, add `::/0`: `--allowed-ips=0.0.0.0/0,::/0`.

> [!WARNING]
> Allowlisting is fail-closed: if not configured, all requests are rejected. This applies to all access paths: `/admin/`, `/manage/`, SSH password authentication, and SSH local port forwarding.

### Reverse proxy support

By default, `--allowed-ips` is matched against the direct TCP peer address of each request. If the server sits behind a reverse proxy or load balancer, every request's peer address is the proxy's own IP. The allowlist then either blocks every real client, or (if the proxy's IP is added to the allowlist to make things work) effectively allows anyone reachable through that proxy, defeating the allowlist either way.

`--trusted-proxies` fixes this for the `/admin/` and `/manage/` HTTP endpoints: `X-Forwarded-For` is only trusted when the immediate peer is itself one of the configured trusted proxies, and the real client IP is then taken from the rightmost entry in that header which isn't itself a trusted proxy. This correctly handles a chain of several trusted hops (e.g. CDN → load balancer → Goauld) and is resistant to a client prepending spoofed entries ahead of the real trusted hops.

#### Flags

- `--trusted-proxies=10.0.0.1,172.16.0.0/12`

Same format as `--allowed-ips`: individual IPs or CIDR ranges. Must include the reverse proxy's own IP/CIDR. Once configured, the proxy's IP itself is never matched against `--allowed-ips`, only whatever it puts in `X-Forwarded-For` is.

> [!NOTE]
> Leave `--trusted-proxies` unset (the default) for a server directly exposed to the internet. Behavior is then unchanged from before: `X-Forwarded-For` is never consulted, and `--allowed-ips` is matched against the direct TCP peer address.

> [!WARNING]
> `--trusted-proxies` only applies to the `/admin/` and `/manage/` HTTP endpoints. SSH password authentication and SSH local port forwarding always use the raw TCP connection's peer address; there is no `X-Forwarded-For` equivalent for a raw SSH connection. If the SSH listener itself is reachable through a reverse proxy or load balancer, `--allowed-ips` will see that proxy's IP for those two access paths regardless of `--trusted-proxies`. **`--ssh-websocket` (below) is the actual fix for reaching SSH through such a proxy**: it doesn't make `--trusted-proxies` apply to raw SSH, it gives SSH traffic its own HTTP(S)-tunneled path that `--trusted-proxies` *does* cover.

### SSH over WebSocket

For SSH access (as opposed to `/admin/`/`/manage/`), there is no way to make a raw TCP connection carry a real client IP through a reverse proxy: `--trusted-proxies` cannot help it directly. Instead, `--ssh-websocket` lets SSH traffic tunnel through an HTTP(S) WebSocket endpoint, `/ssh-ws/`, which *is* one of the HTTP endpoints `--allowed-ips`/`--trusted-proxies` gates. Once inside the tunnel, the connection is handed off directly to the same sshd instance direct-TCP clients use, same password auth, same local-port-forwarding-to-agent mechanism, so nothing about the SSH protocol itself changes.

#### Flags

- `--ssh-websocket`

Disabled by default. Also gated by `--access-token` (the same token used for `/manage/`) in addition to `--allowed-ips`/`--trusted-proxies`.

> [!NOTE]
> Direct raw-TCP SSH access on `--sshd-listen-addr` keeps working unchanged whether or not `--ssh-websocket` is enabled: this is an additional path in, not a replacement.

See [SSH]({{< ref "04-client/03-ssh" >}}#connecting-over-websocket) for the corresponding client-side `--ws` flag.

## Access token

> [!WARNING]
> Access tokens are fail-closed like IP allowlisting: leaving `--access-token` or `--admin-token` unset does not disable authentication, it rejects every request to the corresponding endpoints instead.

### User access token


The user access token restricts:
- The `/manage/` endpoints


#### Flags

- `--access-token=token1,token2`


> [!NOTE]
> Multiple access tokens can be provided, each independently issuable and revocable. All tokens grant identical access; there is no per-token scoping or segmentation of what a given token can do.


### Admin access token

The admin access token restricts:
- The `/admin/` endpoints


#### Flags

- `--admin-token=token1,token2`

## Admin token embedding

Access tokens cannot contain `:` (colon), which is reserved as a separator. This restriction only applies to access tokens; admin tokens may contain colons, since the full value after the first colon is always used as the admin token.

To include both access and admin tokens in a single Authorization header, format them as `access-token:admin-token`. The `POST /manage/agent/{id}/kill` endpoint supports this format:

```http
Authorization: <access-token>:<admin-token>
```

## API reference

### `/manage/` endpoints

Protected by the user access token (see [Access token](#access-token)).

| Method | Route                          | Purpose                                             |
| ------ | ------------------------------- | ---------------------------------------------------- |
| POST   | `/manage/agent/{id}/kill`       | Kill an agent (accepts the embedded admin token, see above) |
| GET    | `/manage/agent/{id}`            | Get information about an agent by ID                |
| DELETE | `/manage/agent/{id}`            | Delete an agent and close its remaining connections |
| GET    | `/manage/agent/by_name/{name}`  | Get information about an agent by name              |
| GET    | `/manage/agent/`                | List all agents                                     |
| POST   | `/manage/clearport/`            | Clear remaining connections for a port or an agent  |
| GET    | `/manage/version/`              | Get the server version                              |
| POST   | `/manage/agent/{id}/setClipboard` | Set the agent's clipboard content (deprecated, but still reachable) |
| POST   | `/manage/agent/{id}/getClipboard` | Read the agent's clipboard content (deprecated, but still reachable) |
| POST   | `/manage/agent/{id}/addWGPeer`  | Add a WireGuard peer to the agent (deprecated, but still reachable) |

### `/admin/` endpoints

Protected by the admin token (see [Admin access token](#admin-access-token)).

| Method | Route                | Purpose                                             |
| ------ | --------------------- | ---------------------------------------------------- |
| GET    | `/admin/config/`      | Get the running configuration (sanitized)           |
| GET    | `/admin/dump/`        | Dump information for all agents                     |
| GET    | `/admin/state/`       | Get the full server state (config and agents)       |
| GET    | `/admin/dump/{id}`    | Dump information for a single agent                 |
| POST   | `/admin/loglevel/{level}` | Change the server's log level                    |

