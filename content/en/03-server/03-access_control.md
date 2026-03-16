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
> IF the server runs in a docker environment, the deployment should ensure that the remote IP address is correctly forwarded to the server

### Flag

- `--allowed-ips=192.168.1.1,192.168.2.0/24`

## Access token

### User access token


The user access token restricts:
- The `/manage/` endpoints


#### Flag

- `--access-token=token1,token2`


> [!NOTE]
> Multiple access tokens can be provided to segment user access.


### Admin access token

The admin access token restricts:
- The `/admin/` endpoints


#### Flag

- `--admin-token`

